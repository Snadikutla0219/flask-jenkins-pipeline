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
                sh 'docker rm -f flask-health-app-running || true'
                sh 'docker run -d --name flask-health-app-running -p 5000:5000 flask-health-app'
                sh 'sleep 3'
                sh 'curl -f http://localhost:5000/health'
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
