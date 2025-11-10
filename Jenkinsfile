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
        
        stage('Create Environment File') {
            steps {
                script {
                    sh '''
                    echo "VITE_SUPABASE_URL=https://prhsliwzscnekhpbnwnq.supabase.co" > .env
                    echo "VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InByaHNsaXd6c2NuZWtocGJud25xIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjI3MDI3OTksImV4cCI6MjA3ODI3ODc5OX0.4XwTD491YO55bad5--ywf5RJqnGZuEkULkluaHfVneU" >> .env
                    '''
                    sh 'cat .env'
                }
            }
        }
        
        stage('Build and Deploy') {
            steps {
                script {
                    sh 'docker-compose -f docker-compose-jenkins.yml down || true'
                    sh 'docker build -t blog-app-jenkins .'
                    sh 'docker-compose -f docker-compose-jenkins.yml up -d'
                }
            }
        }
        
        stage('Verify') {
            steps {
                script {
                    sleep time: 30, unit: 'SECONDS'
                    sh '''
                      echo "=== Container Status ==="
                      docker ps
                      echo "=== Application Logs ==="
                      docker logs blog-app-jenkins || true
                      echo "=== Testing Connection ==="
                      curl -f http://localhost:3001 && echo "SUCCESS" || echo "CONTINUING - App might need more time"
                    '''
                }
            }
        }
    }
}
            
