pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t cicd-exp8 .'
            }
        }

        stage('Test Application') {
            steps {
                sh '''
                docker rm -f test-container || true
                docker run -d -p 8081:80 --name test-container cicd-exp8
                sleep 5
                curl -f http://localhost:8081
                docker stop test-container
                docker rm test-container
                '''
            }
        }

        stage('Deploy to Dev') {
            steps {
                sh '''
                docker rm -f dev-container || true
                docker run -d -p 8080:80 --name dev-container cicd-exp8
                '''
            }
        }

        stage('Deploy to Prod') {
            steps {
                sh '''
                docker rm -f prod-container || true
                docker run -d -p 9090:80 --name prod-container cicd-exp8
                '''
            }
        }
    }
}
