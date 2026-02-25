pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Polaris SCA Scan') {
            steps {
                sh '''
                curl -Ls https://polaris.blackduck.com/cli/latest/bridge.sh | bash -s -- \
                --server-url=https://polaris.blackduck.com \
                --access-token=${POLARIS-TOKEN} \
                --assessment-types=SCA \
                --sca-types=SCA-PACKAGE,SCA-SIGNATURE
                '''
            }
        }
    }
}
