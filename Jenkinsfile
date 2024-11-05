pipeline {
    agent any

    environment {
        KUBECONFIG = '/etc/rancher/k3s/k3s.yaml'
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
                    /var/jenkins_home/helm repo add bitnami https://charts.bitnami.com/bitnami
                    /var/jenkins_home/helm repo update
                    /var/jenkins_home/helm upgrade --install wordpress ./wordpress-chart --namespace wordpress --create-namespace
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
