pipeline {
    agent {
        label 'AGENT-1' 
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    environment {
        def appVersion = '' //declare global variable here so that this can be used across all stages
    }
    parameters {
        string(name: 'appVersion', defaultValue: '1.0.0', description: 'What is the application version?') // these values we get from backend-using-jenkins project
    }
    stages {
        stage('print the version') {
            steps {
                echo "${params.appVersion}"
            }
        }
        stage('Init') {
            steps {
                sh """
                    cd terraform
                    terraform init

                """
            }
        }
        stage('Plan') {
            steps {
                sh """
                    pwd
                    cd terraform
                    terraform plan -var="app_version=${params.appVersion}"

                """
            }
        }
        stage('Deploy') {
            steps {
                sh """
                    pwd
                    cd terraform
                    terraform apply -auto-approve -var="app_version=${params.appVersion}"
                """
            }
        }
        
    }

    post {
        always {
            echo "I will always run even when pipeline is success or failure"
            deleteDir()
        }
        success {
            echo "I will run when pipeline is success"
        }
        failure {
            echo "I will run when pipeline is failure"
        }
    }
}