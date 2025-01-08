pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'media-streaming-app'
        REGISTRY_URL = 'https://icr.io/api'
        CLUSTER_NAME = 'stream-dev'
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/sharath3002/media-streaming-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $REGISTRY_URL/$DOCKER_IMAGE .'
                }
            }
        }
        stage('Push Docker Image to IBM Cloud Container Registry') {
            steps {
                script {
                    sh 'docker push $REGISTRY_URL/$DOCKER_IMAGE'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh '''
                    ibmcloud login --apikey m5KKzGAsxGjzkdFCACDADZTib359QwVKEfZG6mm76bYZ -r in-che -g default
                    ibmcloud ks cluster config --cluster $CLUSTER_NAME
                    kubectl apply -f k8s/deployment.yaml
                    '''
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
