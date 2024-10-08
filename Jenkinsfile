pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/ihebja/TEST2.git' // Remplacez par votre repository
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Quality Check') {
            steps {
                script {
                    def qualityGate = sh(script: 'mvn sonar:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.login=<sonar_token>', returnStatus: true)
                    if (qualityGate != 0) {
                        error 'Quality gate failed!'
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Ajoutez votre logique de déploiement ici
            }
        }
    }
    post {
        success {
            mail to: 'iheb.eljani@gmail.com',
                subject: "Build Successful: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Great! Build completed successfully."
        }
        failure {
            mail to: 'iheb.eljani@gmail.com',
                subject: "Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                body: "Build failed. Please check the logs."
        }
    }
}
