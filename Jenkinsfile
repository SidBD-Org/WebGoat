pipeline {
    agent any
    tools { 
        jdk 'JDK17' 
        maven 'maven-3.9.11'
    } 

    environment {
        POLARIS_TOKEN = credentials('prdPolarisTKN-Sid')
    }

        stage('Polaris SCA Scan') {
            steps {
                sh './mvnw clean install -DskipTests'
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
                    --assessment-types=SAST,SCA
                '''
            }
        }
    }
}
``
