pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Spécifie explicitement la branche "main"
                git branch: 'main', url: 'https://github.com/abdelillah-fixer/Projet-DevOps-BenjouAbdelilah.git'
            }
        }
        stage('Build') {
            steps {
                dir('demo') {
                    sh 'mvn clean package'
                }
            }
        }
        stage('Test') {
            steps {
                dir('demo') {
                    sh 'mvn test'
                }
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'demo/target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
            when {
                expression { currentBuild.result == null || currentBuild.result == 'SUCCESS' }
            }
            steps {
                echo 'Deploying application...'
                // Exemple simple : copie du jar (adapter selon ton environnement)
                // sh 'cp demo/target/*.jar /path/to/deploy'
            }
        }
        stage('Notify Slack') {
            steps {
                slackSend channel: '#devops', message: "Build ${env.BUILD_NUMBER} completed successfully"
            }
        }
    }
}
