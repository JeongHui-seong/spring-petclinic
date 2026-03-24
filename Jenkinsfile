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
        stage('Docker Build && Push') {
            steps{
                sh '''
                docker build -t ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} .
                docker tag ${DOCKER_IMAGE_NAME}:${BUILD_NUMBER} hshs99/${DOCKER_IMAGE_NAME}:latest
                echo ${DOCKERHUB_CRED_PSW} | docker login -u ${DOCKERHUB_CRED_USR} --password-stdin
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
                sshPublisher(publishers: [sshPublisherDesc(configName: 'target',
                transfers: [sshTransfer(cleanRemote: false,
                excludes: '',
                execCommand: '''
                docker rm -f $(docker ps -aq)
                docker rmi -f $(docker images -q)
                docker run -itd -p 80:8080 --name spring-petclinic hshs99/spring-petclinic:latest
                ''',
                execTimeout: 120000,
                flatten: false,
                makeEmptyDirs: false,
                noDefaultExcludes: false,
                patternSeparator: '[, ]+',
                remoteDirectory: '',
                remoteDirectorySDF: false,
                removePrefix: 'target',
                sourceFiles: '')],
                usePromotionTimestamp: false,
                useWorkspaceInPromotion: false,
                verbose: false)])
            }
        }
    }
}
