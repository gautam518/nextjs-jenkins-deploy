pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'gautam518/nextjs-app1:latest'
        KUBE_CONFIG = credentials('kubeconfig')  // Optional: if using kubeconfig as a secret file
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/your/repository.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-cred') {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Use kubectl directly (assumes ~/.kube/config is accessible in Jenkins)
                    sh '''
                    kubectl apply -f nextjs-deploy.yaml
                    '''
                }
            }
        }
    }
}

