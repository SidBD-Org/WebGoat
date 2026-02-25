pipeline {
    agent any

    environment {
        // Your exact credential ID for Polaris (Secret Text)
        POLARIS_TOKEN            = credentials('Sid-PolarisTkn')

        // Polaris settings
        POLARIS_SERVER_URL       = 'https://polaris.blackduck.com'
        POLARIS_APPLICATION_NAME = 'WebGoat'
        POLARIS_PROJECT_NAME     = 'WebGoat'
        POLARIS_BRANCH_NAME      = 'jenkins'
    }

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build') {
            steps {
                script {
                    // Resolve tool install directories defined under "Global Tool Configuration"
                    def JDK_HOME    = tool name: 'openjdk-17',    type: 'jdk'
                    def MAVEN_HOME  = tool name: 'maven-3.9.11', type: 'maven'

                    withEnv([
                        "JAVA_HOME=${JDK_HOME}",
                        "PATH+JAVA=${JDK_HOME}/bin",
                        "PATH+MAVEN=${MAVEN_HOME}/bin"
                    ]) {
                        sh 'mvn -v && java -version'
                        sh 'mvn -B package'
                    }
                }
            }
        }

        stage('Polaris') {
            steps {
                // Black Duck Security Scan Jenkins plugin (Polaris mode)
                security_scan product: 'polaris',
                    polaris_server_url:        "${POLARIS_SERVER_URL}",
                    polaris_access_token:      "${POLARIS_TOKEN}",
                    polaris_assessment_types:  'SAST,SCA',
                    polaris_application_name:  "${POLARIS_APPLICATION_NAME}",
                    polaris_project_name:      "${POLARIS_PROJECT_NAME}",
                    polaris_branch_name:       "${POLARIS_BRANCH_NAME}",
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
