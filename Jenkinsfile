pipeline {
    agent {
        label 'AGENT-1'
    }

    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
    }

    environment {
        DEBUG = 'true'
        appVersion = ''  // global variable
    }

    stages {

        stage('Read the version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "App version: ${appVersion}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Docker build') {
            steps {
                sh """
                    docker build -t ramak8sgcp/backend:${appVersion} .
                    docker images
                """
            }
        }

        stage('Print Params') {
            steps {
                echo "Hello ${params.PERSON}"
                echo "Biography: ${params.BIOGRAPHY}"
                echo "Toggle: ${params.TOGGLE}"
                echo "Choice: ${params.CHOICE}"
                echo "Password: ${params.PASSWORD}"
            }
        }

        stage('Approval') {
            when { expression { return true } }  
            input {
                message "Should we continue?"
                ok "Yes, continue."
                submitter "alice,bob"
                parameters {
                    string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
                }
            }
            steps {
                echo "Hello, ${PERSON}, nice to meet you."
            }
        }

    } // end of stages

    post {
        always {
            echo "This section runs always"
            deleteDir()
        }
        success {
            echo "This section runs on success"
        }
        failure {
            echo "This section runs on failure"
        }
    }
}
