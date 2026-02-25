// Uses the Black Duck Security Scan Jenkins plugin with Polaris
// Plugin docs: https://www.jenkins.io/doc/pipeline/steps/blackduck-security-scan/
// Polaris + plugin usage: https://documentation.blackduck.com/bundle/bridge/page/documentation/security_scan_for_polaris.html

pipeline {
    agent any

    environment {
        // Your exact credential IDs:
        // Polaris access token (Secret text)
        POLARIS_TOKEN = credentials('Sid-PolarisTkn')

        // Server & naming
        POLARIS_SERVER_URL       = 'https://polaris.blackduck.com'
        POLARIS_APPLICATION_NAME = 'WebGoat'
        POLARIS_PROJECT_NAME     = 'WebGoat'
    }

    tools {
        maven 'maven-3'
        jdk   'openjdk-21'
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build') {
            steps {
                sh 'mvn -B package'
            }
        }

        // Always run Polaris on whatever branch Jenkins checked out (e.g., "jenkins")
        stage('Polaris') {
            steps {
                security_scan product: 'polaris',
                    polaris_server_url:      "${POLARIS_SERVER_URL}",
                    polaris_access_token:    "${POLARIS_TOKEN}",
                    polaris_assessment_types:'SAST,SCA',
                    polaris_application_name:"${POLARIS_APPLICATION_NAME}",
                    polaris_project_name:    "${POLARIS_PROJECT_NAME}",
                    polaris_branch_name:     "${env.BRANCH_NAME}",
                    // Reports & behavior
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
