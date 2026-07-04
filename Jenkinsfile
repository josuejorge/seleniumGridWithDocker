pipeline{

    agent none

    stages{

        stage('Build Jar'){
            agent {
                docker {
                    image 'maven:3.9-eclipse-temurin-21'
                    args '-v $HOME/.m2:/root/.m2'
                }
            }
            steps{
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Image'){
            agent {
                docker {
                    image 'docker:24-cli'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps{
                sh 'docker build -t josuejorge/selenium:latest .'
            }
        }

        stage('Push Image'){
            agent {
                docker {
                    image 'docker:24-cli'
                    args '-v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            environment{
                DOCKER_HUB = credentials('dockerhub-creds')
            }
            steps{
                sh 'echo ${DOCKER_HUB_PSW} | docker login -u ${DOCKER_HUB_USR} --password-stdin'
                sh 'docker push josuejorge/selenium:latest'
                sh "docker tag josuejorge/selenium:latest josuejorge/selenium:${env.BUILD_NUMBER}"
                sh "docker push josuejorge/selenium:${env.BUILD_NUMBER}"
            }
            post {
                always {
                    sh 'docker logout'
                }
            }
        }

    }

}
