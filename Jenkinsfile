pipeline {
    agent any
    environment {
        KUBECONFIG = '/etc/rancher/k3s/k3s.yaml'
        HELM_PATH = sh(script: 'which helm', returnStdout: true).trim()
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/18bitmood/rsschool-wordpress.git'
            }
        }
        stage('Deploy WordPress') {
            steps {
                sh '''
                    ${HELM_PATH} repo add bitnami https://charts.bitnami.com/bitnami
                    ${HELM_PATH} repo update
                    ${HELM_PATH} upgrade --install wordpress ./wordpress-chart --namespace wordpress --create-namespace
                '''
            }
        }
        stage('Verify Deployment') {
            steps {
                sh '''
                    kubectl get pods -n wordpress
                    kubectl get svc -n wordpress
                '''
            }
        }
    }
}
