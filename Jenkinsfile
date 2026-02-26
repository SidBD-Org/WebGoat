pipeline {
    agent any
    tools { jdk 'JDK17' }  // <-- The name you configured in Global Tool Configuration
    environment {
        POLARIS_TOKEN = credentials('prdPolarisTKN-Sid')
    }
    stages {
        stage('Set JAVA_HOME') {
            steps {
                withEnv(["JAVA_HOME=${tool 'JDK17'}", "PATH=${tool 'JDK17'}/bin:${env.PATH}"]) {
                    sh 'java -version'
                }
            }
        }

        // ... your Checkout stage ...

        stage('Polaris SCA Scan') {
            steps {
                withEnv(["JAVA_HOME=${tool 'JDK17'}", "PATH=${tool 'JDK17'}/bin:${env.PATH}"]) {
                    sh '''
                    echo "Downloading Polaris Bridge CLI..."
                    curl -fL -o bridge.zip "https://sig-repo.synopsys.com/artifactory/bds-integrations-release/com/synopsys/integration/synopsys-bridge/latest/synopsys-bridge-linux64.zip"
                    unzip -o bridge.zip
                    chmod +x synopsys-bridge-linux64/bridge
                    echo "Running Polaris Scan..."
                    ./synopsys-bridge-linux64/bridge \
                      --server-url=https://polaris.blackduck.com \
                      --access-token=$POLARIS_TOKEN \
                      --assessment-types=SCA
                    '''
                }
            }
        }
    }
}
