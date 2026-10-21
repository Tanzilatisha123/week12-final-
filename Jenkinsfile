pipeline {
    agent any

    environment {
        PYTHON = 'C:\\Users\\Tanzila\\AppData\\Local\\Python\\bin\\python.exe'
    }

    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                // Install dependencies
                bat "\"%PYTHON%\" -m pip install -r requirements.txt"

                // Start Flask app in background
                bat "start /B \"\" \"%PYTHON%\" app.py"

                // Wait for app startup
                bat "timeout /t 10 /nobreak"

                // Run tests
                bat "\"%PYTHON%\" -m pytest -v"
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image"
                bat "docker build -t seleniumdemoapp:v1 ."
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Pushing Docker Image"

                bat "docker tag seleniumdemoapp:v1 tanzila123/sample2026:seleniumtestimage"
                bat "docker push tanzila123/sample2026:seleniumtestimage"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat "kubectl apply -f deployment.yaml --validate=false"
                bat "kubectl apply -f service.yaml"
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
