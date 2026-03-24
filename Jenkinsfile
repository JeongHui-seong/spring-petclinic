pipeline {
    agent any
    
    tools {
        jdk 'JDK21'
        maven 'M3'
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
                echo '1'
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
