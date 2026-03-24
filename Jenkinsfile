pipeline {
    agent any
    
    tools {
        jdk 'JDK21'
        maven 'M3'
    }
    environment {
        // 환경변수 지정
        DOCKER_IMAGE_NAME = 'spring-petclinic'

        // Credentials
        // DOCKERHUB_CRED = credentials('dockerCredentials')
    }
    
    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/JeongHui-seong/spring-petclinic.git',
                branch: 'main'
            }
        }
        stage('Maven Build') {
            steps{
                sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }
        stage('Docker Image Create') {
            steps{
                sh '''
                docker build -t ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} .
                docker tag ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} hshs99/${DOCKER_IMAGE_NAME}:latest
                '''
            }
        }
        stage('Docker Hub Login') {
            steps{
                echo '1'
            }
        }
        stage('Docker Image Push') {
            steps{
                echo '1'
            }
        }
        stage('Docker Container Run') {
            steps{
                echo '1'
            }
        }
    }
}
