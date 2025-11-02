pipeline {
    agent any
    
    stages {
        stage('Setup Docker') {
            steps {
                echo "Setup Docker stage is running"
                sh '''
                    sudo systemctl start docker || true
                    sudo chmod 666 /var/run/docker.sock || true
                    docker --version
                '''
            }
        }
        
        stage('Build') {
            steps {
                echo "Build stage is running"
                sh 'docker-compose build'
            }
        }
        
        stage('Test') {
            steps {
                echo "Test stage is running"
                sh 'docker-compose up -d'
                sh 'sleep 30'
                sh 'docker-compose down'
            }
        }
        
        stage('Deploy') {
            steps {
                echo "Deploy stage is running"
                sh 'docker-compose up -d'
            }
        }
    }
    
    post {
        always {
            echo "Pipeline execution completed"
            sh 'docker-compose down || true'
        }
    }
}
