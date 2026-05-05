stage('Deploy') {
    steps {
        echo 'Deploying Flask app container...'
        sh '''
            set -e

            if docker ps -a --format '{{.Names}}' | grep -q '^flask-health-app-running$'; then
                docker rm -f flask-health-app-running
            fi

            docker run -d --name flask-health-app-running -p 5000:5000 flask-health-app

            echo "Container logs:"
            docker logs flask-health-app-running || true

            echo "Waiting for app to start..."

            for i in {1..10}; do
                if curl -f http://localhost:5000/health; then
                    echo "App is up!"
                    exit 0
                fi
                sleep 2
            done

            echo "App failed to start"
            docker logs flask-health-app-running || true
            exit 1
        '''
    }
}