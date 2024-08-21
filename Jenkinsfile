pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/lvimuth/CI-CD-Pipeline'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t lvimuth/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'nodejs-test-password', variable: 'nodejs-test-password')]) {
                    script {
                        bat "docker login -u lvimuth -p ${nodejs-test-password}"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push lvimuth/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
