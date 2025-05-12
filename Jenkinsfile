pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'gautam518/nextjs-app1:latest'
        KUBECONFIG = '/var/jenkins_home/.kube/config'
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
                    echo "Applying Kubernetes manifests using kubeconfig at $KUBECONFIG"
                    sh '''
                    kubectl version --short
                    kubectl apply -f nextjs-deploy.yaml
                    kubectl get pods
                    '''
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}

