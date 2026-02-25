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
                polaris scan \
                --server-url=https://polaris.blackduck.com \
                --access-token=${POLARIS_TOKEN} \
                --assessment-types=SCA \
                --sca-types=SCA-PACKAGE,SCA-SIGNATURE
                '''
            }
        }
    }
}
