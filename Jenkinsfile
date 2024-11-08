pipeline {
    agent any
    environment {
        KUBECONFIG = '/etc/rancher/k3s/k3s.yaml'
    }
    stages {
        stage('Install Helm') {
            steps {
                sh '''
                    export PATH=$PATH:/var/jenkins_home/bin
                    mkdir -p /var/jenkins_home/bin
                    cd /var/jenkins_home
                    curl -LO https://get.helm.sh/helm-v3.12.3-linux-amd64.tar.gz
                    tar -zxvf helm-v3.12.3-linux-amd64.tar.gz
                    mv linux-amd64/helm /var/jenkins_home/bin/
                '''
            }
        }
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/18bitmood/rsschool-wordpress.git'
            }
        }
        stage('Deploy WordPress') {
            steps {
                sh '''
                    export PATH=$PATH:/var/jenkins_home/bin
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
