pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building Docker image...'
                sh 'docker build -t flask-health-app .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running container tests...'
                sh 'docker run --rm flask-health-app pytest test_app.py -v'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Flask app container...'
                sh '''
                    set -e
                    if docker ps -a --format '{{.Names}}' | grep -q '^flask-health-app-running$'; then
                        docker rm -f flask-health-app-running || true
                    fi
                    docker run -d --name flask-health-app-running -p 5000:5000 flask-health-app
                    sleep 3
                    curl -f http://localhost:5000/health
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished: cleaning up and logging status.'
        }
        success {
            echo 'Pipeline succeeded: app built, tested, and deployed successfully.'
        }
        failure {
            echo 'Pipeline failed: check the build output and fix any issues.'
        }
    }
}
