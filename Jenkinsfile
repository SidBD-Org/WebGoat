pipeline {
    agent any

    environment {
        POLARIS_TOKEN            = credentials('Sid-PolarisTkn')
        POLARIS_SERVER_URL       = 'https://polaris.blackduck.com'
        POLARIS_APPLICATION_NAME = 'WebGoat'
        POLARIS_PROJECT_NAME     = 'WebGoat'
        POLARIS_BRANCH_NAME      = 'jenkins'
    }

    tools {
        maven 'maven-3.9.11'
        jdk   'openjdk-17'
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build') {
            steps { sh 'mvn -B package' }
        }

        stage('Polaris') {
            steps {
                security_scan product: 'polaris',
                    polaris_server_url:       "${POLARIS_SERVER_URL}",
                    polaris_access_token:     "${POLARIS_TOKEN}",
                    polaris_assessment_types: 'SAST,SCA',
                    polaris_application_name: "${POLARIS_APPLICATION_NAME}",
                    polaris_project_name:     "${POLARIS_PROJECT_NAME}",
                    polaris_branch_name:      "${POLARIS_BRANCH_NAME}",
                    polaris_reports_sarif_create: true,
                    mark_build_status: 'UNSTABLE',
                    include_diagnostics: false
            }
        }
    }

    post {
        always {
            archiveArtifacts allowEmptyArchive: true, artifacts: '.bridge/bridge.log, .bridge/*/idir/build-log.txt'
            cleanWs()
        }
    }
}
