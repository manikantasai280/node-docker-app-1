pipeline {
    agent any

    stages {

        stage('Checkout from GitHub') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/manikantasai280/node-docker-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t node-docker-app:${BUILD_NUMBER} .
                docker tag node-docker-app:${BUILD_NUMBER} manikantasai280/node-docker-app:${BUILD_NUMBER}
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker push manikantasai280/node-docker-app:0.0.1'
                }
            }
        }
        
        stage('Create container') {
            steps {
                sh 'docker run -d -p 3000:8080 manikantasai280/node-docker-app:${BUILD_NUMBER}'
            }
        }



    }
}
