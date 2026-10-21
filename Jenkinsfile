pipeline {
    agent any


    stages {

      stage('Run Selenium Tests with pytest') {
    steps {
        echo "Running Selenium Tests using pytest"

        bat '"C:\\Users\\Tanzila\\AppData\\Local\\Python\\bin\\python.exe" -m pip install -r requirements.txt'

        bat 'start /B "" "C:\\Users\\Tanzila\\AppData\\Local\\Python\\bin\\python.exe" app.py'

        bat '"C:\\Users\\Tanzila\\AppData\\Local\\Python\\bin\\python.exe" -c "import time; time.sleep(10)"'

        bat '"C:\\Users\\Tanzila\\AppData\\Local\\Python\\bin\\python.exe" -m pytest -v'
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
