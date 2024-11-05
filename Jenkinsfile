pipeline {
    agent any
    
    environment {
        KUBECONFIG = '/etc/rancher/k3s/k3s.yaml'
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/18bitmood/rsschool-wordpress.git'
            }
        }
        
        stage('Deploy WordPress') {
            steps {
                sh '''
                    helm repo add bitnami https://charts.bitnami.com/bitnami
                    helm repo update
                    helm upgrade --install wordpress ./wordpress-chart --namespace wordpress --create-namespace
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
