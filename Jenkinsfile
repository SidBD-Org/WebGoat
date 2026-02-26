pipeline {
    agent any

    environment {
        // Must match exactly your Jenkins credential ID
        POLARIS_TOKEN = credentials('prdPolarisTKN-Sid')
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

                curl -fLsS -o bridge.zip $BRIDGECLI_LINUX64 && unzip -qo -d $WORKSPACE_TMP bridge.zip && rm -f bridge.zip

                chmod +x bridge.sh

                echo "Running Polaris Scan..."

                bash bridge.sh \
                --server-url=https://polaris.blackduck.com \
                --access-token=$POLARIS_TOKEN \
                --assessment-types=SCA
                '''
            }
        }
    }
}
