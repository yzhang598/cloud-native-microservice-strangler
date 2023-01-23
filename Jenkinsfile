def strangler_images = ["legacy-edge","customer-service","edge-service","profile-web","discovery-service","config-service","user-service","profile-service"]

pipeline {
    agent {label 'jenkins_node1'}
    

    environment {
        legacyedge =''
        customerservice = ''
        registryCredential = 'darsanantra-dockerhub'

    }
    
    stages {
        stage('Checkout Git') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/yzhang598/cloud-native-microservice-strangler.git']]])
            }
        }
        stage('Build') {
            steps {
                script {
				    sh "mvn clean install"
                }
            }
        }
        stage ('Tag Docker Image') {
            steps {
                script {
				    strangler_images.each { name->
				        sh "docker tag ${name}:latest yzhang598/micro:${name}"
                }
            }
        }

        }
        
        stage ('Upload Image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        strangler_images.each { name->
                            sh "docker push yzhang598/mircro:${name}"
                        }
                    }
                }
            }
        }
  
    }
}
