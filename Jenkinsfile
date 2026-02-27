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

        stage('Verify Java 25') {
            steps {
                sh 'echo "JAVA_HOME=$JAVA_HOME"'
                sh 'java -version'
            }
        }

        stage('Debug JDK Folder') {
            steps {
                sh 'echo "Listing JDK tool directory..."'
                sh 'ls -l /var/lib/jenkins/tools/hudson.model.JDK/'
                sh 'ls -l /var/lib/jenkins/tools/hudson.model.JDK/openjdk-25'
            }
        }

        stage('Build Application') {
            steps {
                sh './mvnw clean install -DskipTests'
            }
        }

        stage('Download Polaris Bridge CLI') {
            steps {
                sh '''
                    curl -L -o bridge.zip https://repo.blackduck.com/bds-integrations-release/com/synopsys/integration/bridge-cli/latest/bridge-cli-linux64.zip
                    unzip -o bridge.zip
                    chmod +x bridge-cli*/bridge-cli
                '''
            }
        }

        stage('Run Polaris Scan (SAST + SCA)') {
            steps {
                sh '''
                    ./bridge-cli*/bridge-cli \
                    --server-url=https://polaris.blackduck.com \
                    --access-token=$POLARIS_TOKEN \
                    --assessment-types=SAST,SCA
                '''
            }
        }
    }
}
