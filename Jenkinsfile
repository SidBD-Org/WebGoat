pipeline {
    agent any

    tools {
        jdk 'openjdk-25'
        maven 'maven-3.9.11'
    }

    environment {
        POLARIS_TOKEN = credentials('prdPolarisTKN-Sid')
    }

    stages {

        stage('Verify Java Version') {
            steps {
                sh 'java -version'
                sh 'echo $JAVA_HOME'
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean install -DskipTests'
            }
        }

        stage('Polaris Scan') {
            steps {
                sh '''
                    curl -L -o bridge.zip https://repo.blackduck.com/bds-integrations-release/com/synopsys/integration/bridge-cli/latest/bridge-cli-linux64.zip
                    unzip -o bridge.zip
                    chmod +x bridge-cli*/bridge-cli

                    ./bridge-cli*/bridge-cli \
                    --server-url=https://polaris.blackduck.com \
                    --access-token=$POLARIS_TOKEN \
                    --assessment-types=SAST,SCA
                '''
            }
        }
    }
}
