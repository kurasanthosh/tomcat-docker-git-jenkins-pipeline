pipeline {
    agent any
    environment {
        registry = 'kurasanthosh/devopsb23'
        registryCredential = 'dockerhub_id'
        dockerImage = ''
        swarmip = '44.204.109.246'
    }
    stages {
        stage('Build java code') {            
            steps {
                script {
                    chmod 777 build.sh
                    sh './build.sh'   
                }
            }
        }
        stage('Building Docker Image') {            
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Push Docker image') {            
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy Docker Container') {
            steps {
                    script {                  
                    sh 'docker -H ${swarmip}:2375 ps'
                    sh 'docker -H ${swarmip}:2375 service rm tomcat || echo \"not a service\"'
                    sh 'docker -H ${swarmip}:2375 service create --name tomcat -p 8000:8080 kurasanthosh/devopsb23:$BUILD_NUMBER'
                    sh 'curl ${swarmip}:8000'
                    }
            }
        }
        
    }
}
