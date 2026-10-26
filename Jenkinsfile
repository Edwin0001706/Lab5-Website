pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate Source') {
            steps {
                bat 'if not exist index.html exit /b 1'
                bat 'if not exist styles.css exit /b 1'
                bat 'if not exist Dockerfile exit /b 1'
                echo 'Website source files are present.'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t lab5-website:latest .'
            }
        }

        stage('Deploy with Docker') {
            steps {
                powershell '''
                    $existing = docker ps -aq --filter "name=^lab5-website$"
                    if ($existing) {
                        docker rm -f lab5-website
                    }
                    docker run -d --name lab5-website -p 8081:80 lab5-website:latest
                '''
            }
        }
    }

    post {
        success {
            echo 'Lab 5 pipeline completed. Open http://localhost:8081 to view the website.'
        }
        failure {
            echo 'Pipeline failed. Review the Console Output for the stage and error message.'
        }
    }
}
