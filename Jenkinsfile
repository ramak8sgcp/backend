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
        appVersion = '' // this will become global we can use across pipeline
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
        // stage('Install Dependencies') {
        //     steps {
        //         sh "npm install"
                
        //     }
        // }
        stage('Deploy') {
            when {
                expression { env.GIT_BRANCH == "origin/main" }
            }
            steps {
                sh "echo this is Deploy"
                // error 'pipeline failed'
            }
        }
    }
    post {
        always {
            echo "This section runs always"
            deleteDir()
        }
        success {
            echo "This section runs when pipeline succeeds"
        }
        failure {
            echo "This section runs when pipeline fails"
        }
    }
}
