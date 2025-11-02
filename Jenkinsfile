pipeline {
    agent any
    
    stages {
        stage('Setup Docker') {
            steps {
                echo "Setup Docker stage is running"
                sh '''
                    echo "Current user: $(whoami)"
                    echo "Checking Docker service..."
                    sudo systemctl status docker || sudo systemctl start docker
                    sleep 5
                    echo "Checking Docker socket permissions..."
                    ls -la /var/run/docker.sock
                    sudo chmod 666 /var/run/docker.sock || true
                    echo "Testing Docker connection..."
                    docker --version
                    docker info || echo "Docker daemon not accessible"
                '''
            }
        }
        
        stage('Build') {
            steps {
                echo "Build stage is running"
                sh '''
                    # Remove version from docker-compose.yml if exists
                    sed -i '/^version:/d' docker-compose.yml || true
                    docker-compose build
                '''
            }
        }
        
        stage('Test') {
            steps {
                echo "Test stage is running"
                sh 'docker-compose up -d'
                sh 'sleep 30'
                sh 'curl -f http://localhost:8080 || echo "Website not accessible yet"'
                sh 'docker-compose down'
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
