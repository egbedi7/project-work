pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t project-work:1.0 .'
            }
        }

        stage('Docker Deploy') {
            steps {
                sh '''
                    docker stop project-work-app || true
                    docker rm project-work-app || true
                    docker run -d \
                      --name project-work-app \
                      -p 8080:8080 \
                      project-work:1.0
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD Pipeline failed!'
        }
    }
}
