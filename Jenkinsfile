pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/pramodkavi/Jenkins_PipeLine_Node'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t kavindupramod/test-node-app:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubPW', variable: 'dockerhubPW')]) {
                    script {
                        bat "docker login -u kavindupramod -p %dockerhubPW%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push kavindupramod/test-node-app:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}