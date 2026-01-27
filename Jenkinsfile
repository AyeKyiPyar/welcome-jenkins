pipeline {
    agent any

    tools {
        maven "maven3.9"
    }

    environment {
        APP_NAME = "welcome-jenkins"
        IMAGE_NAME = "welcome-jenkins:1.0"
        CONTAINER_NAME = "welcome-jenkins-app"
        APP_PORT = "8081"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/AyeKyiPyar/welcome-jenkins.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                sudo docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker run -d \
                  -p $APP_PORT:$APP_PORT \
                  --name $CONTAINER_NAME \
                  $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build & Deployment Successful!'
        }
        failure {
            echo '❌ Build or Deployment Failed!'
        }
    }
}
