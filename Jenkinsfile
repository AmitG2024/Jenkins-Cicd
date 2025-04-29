pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/AmitG2024/Jenkins-Cicd.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Building the app...'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }
        stage('Push to JFrog') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'jfrog-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                        docker login -u $USER -p $PASS yoursubdomain.jfrog.io
                        docker push yoursubdomain.jfrog.io/docker/myapp:latest
                    '''
                }
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
            }
        }
    }
}
