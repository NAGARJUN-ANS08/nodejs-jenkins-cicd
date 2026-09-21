pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/NAGARJUN-ANS08/nodejs-jenkins-cicd.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t nodejs-status-api:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                    docker stop nodejs-status-container || true
                    docker rm nodejs-status-container || true
                    docker run -d --name nodejs-status-container -p 3000:3000 nodejs-status-api:latest
                '''
            }
        }

        stage('Test API') {
            steps {
                sh '''
                    sleep 5
                    curl -f http://localhost:3000/status
                '''
            }
        }
    }

    post {
        always {
            sh 'docker logs nodejs-status-container || true'
        }

        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'CI/CD Pipeline failed!'
        }
    }
}
