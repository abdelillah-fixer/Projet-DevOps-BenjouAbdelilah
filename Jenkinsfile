pipeline {
    agent any

    tools {
        // Le nom "Maven" doit correspondre à celui configuré dans Manage Jenkins → Tools → Maven
        maven 'Maven'
    }

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
                    // Utilise Maven configuré dans Tools
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

       
    }

    post {
        failure {
            slackSend channel: '#devops', message: "Build ${env.BUILD_NUMBER} failed!"
        }
    }
}
