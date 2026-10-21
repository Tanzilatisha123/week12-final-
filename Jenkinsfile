pipeline {
    agent any

    environment {
        PYTHON = 'C:\\Users\\Tanzila\\AppData\\Local\\Python\\bin\\python.exe'
    }

    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                echo "Running Selenium Tests using pytest"

                bat '"%PYTHON%" -m pip install -r requirements.txt'

                bat 'start /B "" "%PYTHON%" app.py'

                bat '"%PYTHON%" -c "import time; time.sleep(10)"'

                bat '"%PYTHON%" -m pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat 'docker build -t seleniumdemoapp:v1 .'
            }
        }

        stage('Docker Login') {
            steps {
                bat 'docker login -u jaya02sri26 -p Jaya@sri26'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                echo "Push Docker Image to Docker Hub"

                bat 'docker tag seleniumdemoapp:v1 jaya02sri26/selenium:seleniumtestimage'

                bat 'docker push jaya02sri26/selenium:seleniumtestimage'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl apply -f deployment.yaml --validate=false'
                bat 'kubectl apply -f service.yaml'
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
