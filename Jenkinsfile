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

        curl -fL -o bridge.zip "https://sig-repo.synopsys.com/artifactory/bds-integrations-release/com/synopsys/integration/synopsys-bridge/latest/synopsys-bridge-linux64.zip"

        echo "Unzipping bridge..."
        unzip -o bridge.zip

        echo "Making bridge executable..."
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
