pipeline {
    agent any
    
    tools {
        jdk 'JDK21'
        maven 'M3'
    }
    environment {
        // 환경변수 지정
        DOCKER_IMAGE_NAME = 'spring-petclinic'
        DOCKER_API_VERSION = '1.43'
        COMPOSE_API_VERSION = '1.43'

        // Credentials
        DOCKERHUB_CRED = credentials('dockerCredentials')
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
                echo "PATH=$PATH"
                which docker
                docker version
                hostname
                docker build -t ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} .
                docker tag ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} hshs99/${DOCKER_IMAGE_NAME}:latest
                '''
            }
        }
        stage('Docker Hub Login') {
            steps{
                sh 'echo ${DOCKERHUB_CRED_PWS} | docker login -u ${DOCKERHUB_CRED_USR} --password-stdin'
            }
        }
        stage('Docker Image Push') {
            steps{
                sh '''
                docker push hshs99/${DOCKER_IMAGE_NAME}:latest
                '''
            }
            post {
                always {
                    sh '''
                    docker image rm -f ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER}
                    docker image rm -f hshs99/${DOCKER_IMAGE_NAME}:latest
                    '''
                }
            }
        }
        stage('Docker Container Run') {
            steps{
                echo '1'
            }
        }
    }
}
