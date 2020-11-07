pipeline {
    agent any
    stages {
        stage('Build Jar') {
            agent {
                any {
                    image 'maven:3-alpine'
                    args '-v $HOME/.m2:/root/.m2'
                }
            }
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }
        stage('Build Image') {
            steps {
                script {
                    powershell	"docker build '-t=22piston/selenium-docker' ."
                }
            }
        }
        stage('Push Image') {
            steps {
                script {
			        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
			        	powershell "docker push 22piston/selenium-docker:'${BUILD_NUMBER}'"
			            powershell "docker push 22piston/selenium-docker:latest"
			        }
                }
            }
        }
    }
}