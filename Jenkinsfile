pipeline {
    agent any
   
    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                // Install Python dependencies
                sh 'pip3 install -r requirements.txt'

                // Start Flask app in background
                sh 'nohup python3 app.py &'

                // Wait a few seconds for the server to start
                sh 'sleep 5'

                // Run tests using pytest
                sh '/Users/akshithareddyk/Library/Python/3.9/bin/pytest -v'

            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                sh "docker build -t seleniumdemoapp:v1 ."
            }
        }

        stage('Docker Login') {
            steps {
                sh 'docker login -u akshithareddy27 -p docker123'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Push Docker Image to Docker Hub"
                sh "docker tag seleniumdemoapp:v1 akshithareddy27/sample:seleniumtestimage"
                sh "docker push akshithareddy27/sample:seleniumtestimage"
            }
        }

        stage('Deploy to Kubernetes') { 
            steps { 
                sh 'kubectl apply -f deployment.yaml --validate=false' 
                sh 'kubectl apply -f service.yaml' 
            } 
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}


