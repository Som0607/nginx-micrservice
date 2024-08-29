pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'som0607/nginx-microservice:latest'
        KUBE_CONFIG_PATH = '/home/som0607/.kube/config'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Som0607/nginx-micrservice.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(DOCKER_IMAGE)
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-creds') {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }
        stage('Deploy to Minikube') {
            steps {
                script {
                    env.KUBECONFIG = KUBE_CONFIG_PATH
                    sh 'kubectl config use-context microjen'
                    sh 'kubectl apply -f k8s/deployment.yaml'
                    sh 'kubectl apply -f k8s/service.yaml'
               }
            }
        }
    }
}

