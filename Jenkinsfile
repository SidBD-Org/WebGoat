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
        sh '''
        echo "Downloading Polaris Bridge CLI..."

        curl -fL -o bridge.zip "https://repo.blackduck.com/bds-integrations-release/com/synopsys/integration/bridge-cli/latest/bridge-cli-linux64.zip"

        echo "Unzipping bridge..."
        unzip -o bridge.zip

        echo "Making bridge executable..."
        chmod +x bridge-cli*/bridge-cli

        echo "Running Polaris Scan..."
        ./bridge-cli*/bridge-cli \
            --server-url=https://polaris.blackduck.com \
            --access-token=$POLARIS_TOKEN \
            --assessment-types=SCA
        '''
    }
}
    }
}
