pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/crispybagel-glitch/blog-app.git'
            }
        }
        
        stage('Build') {
            steps {
                script {
                    sh 'npm install'
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    sh 'docker-compose -f docker-compose.jenkins.yml down || true'
                    sh 'docker-compose -f docker-compose.jenkins.yml up -d'
                }
            }
        }
    }
}
