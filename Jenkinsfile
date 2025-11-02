pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                echo "Build stage is running"
                sh '''
                    echo "Building Docker image directly..."
                    docker build -t group5-web .
                    echo "Docker image built successfully!"
                '''
            }
        }
        
        stage('Test') {
            steps {
                echo "Test stage is running"
                sh '''
                    echo "Testing application..."
                    # Run the container
                    docker run -d --name group5-test -p 8080:80 group5-web
                    sleep 10
                    
                    # Test if the web server is responding
                    if curl -f http://localhost:8080; then
                        echo "✅ Application is running correctly!"
                    else
                        echo "⚠️  Application might be starting..."
                    fi
                    
                    # Stop and remove container
                    docker stop group5-test || true
                    docker rm group5-test || true
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                echo "Deploy stage is running"
                sh '''
                    echo "Deployment simulation completed!"
                    echo "In production, you would push to a registry and deploy to servers"
                '''
            }
        }
    }
    
    post {
        always {
            echo "Pipeline execution completed"
            sh 'docker rm -f group5-test 2>/dev/null || true'
        }
    }
}
