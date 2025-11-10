pipeline {
    agent any
    
    environment {
        DOCKER_HOST = 'unix:///var/run/docker.sock'
        SUPABASE_URL = 'https://prhsliwzscnekhpbnwnq.supabase.co'
        SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InByaHNsaXd6c2NuZWtocGJud25xIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NjI3MDI3OTksImV4cCI6MjA3ODI3ODc5OX0.4XwTD491YO55bad5--ywf5RJqnGZuEkULkluaHfVneU'
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
                    cat > .env << EOF
                    VITE_SUPABASE_URL=${SUPABASE_URL}
                    VITE_SUPABASE_ANON_KEY=${SUPABASE_KEY}
                    EOF
                    '''
                }
            }
        }
        
        stage('Build and Deploy') {
            steps {
                script {
                    sh 'docker-compose -f docker-compose-jenkins.yml down || true'
                    // Build the image manually first
                    sh 'docker build -t blog-app-jenkins .'
                    // Then start without --build flag
                    sh 'docker-compose -f docker-compose-jenkins.yml up -d'
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
            
