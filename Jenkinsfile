pipeline {
    agent any
    
    environment {
        DOCKER_HOST = 'unix:///var/run/docker.sock'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/crispybagel-glitch/blog-app.git'
            }
        }
        
        stage('Build and Deploy') {
            steps {
                script {
                    sh 'docker-compose -f docker-compose-jenkins.yml down || true'
                    sh 'docker-compose -f docker-compose-jenkins.yml up -d --build'
                }
            }
        }
        
        stage('Verify') {
            steps {
                script {
                    sleep time: 30, unit: 'SECONDS'
                    sh 'curl -f http://localhost:3001 || exit 1'
                }
            }
        }
    }
}
            
