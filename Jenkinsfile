pipeline {
    agent any

    environment {
        POLARIS_TOKEN = credentials('Sid-PolarisTkn')
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Polaris SCA Scan') {
            steps {
                sh '''
                echo "Downloading Polaris Bridge CLI..."
                curl -Ls https://polaris.blackduck.com/cli/latest/bridge.sh -o bridge.sh

                echo "Making script executable..."
                chmod +x bridge.sh

                echo "Running Polaris Scan..."
                ./bridge.sh \
                --server-url=https://polaris.blackduck.com \
                --access-token=${POLARIS_TOKEN} \
                --assessment-types=SCA \
                --sca-types=SCA-PACKAGE,SCA-SIGNATURE
                '''
            }
        }
    }
}
