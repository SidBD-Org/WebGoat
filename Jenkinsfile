pipeline {
    agent any

    environment {
        BRIDGE_polaris_accessToken = credentials('Sid-PolarisTkn')
        BRIDGECLI_URL = "https://repo.blackduck.com/bds-integrations-release/com/blackduck/integration/bridge/binaries/bridge-cli-bundle/latest/bridge-cli-bundle-linux64.zip"
        POLARIS_SERVER_URL = "https://polaris.blackduck.com"

        POLARIS_APPLICATION_NAME = "WebGoat"
        POLARIS_PROJECT_NAME     = "WebGoat"
        POLARIS_BRANCH_NAME      = "jenkins"
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Polaris SCA Scan') {
            steps {
                sh '''
                echo "Downloading Bridge CLI..."
                curl -fLsS -o bridge.zip $BRIDGECLI_URL

                echo "Unzipping..."
                unzip -qo bridge.zip -d bridgecli
                chmod +x bridgecli/bridge-cli

                echo "Running Polaris Scan..."
                bridgecli/bridge-cli --stage polaris \
                  polaris.serverUrl=$POLARIS_SERVER_URL \
                  polaris.application.name=$POLARIS_APPLICATION_NAME \
                  polaris.project.name=$POLARIS_PROJECT_NAME \
                  polaris.branch.name=$POLARIS_BRANCH_NAME \
                  polaris.assessment.types=SCA \
                  polaris.sca.types=SCA-PACKAGE,SCA-SIGNATURE
                '''
            }
        }
    }
}
