pipeline {
    agent { label 'slave' }
    
   

    stages {
        stage('Checkout Git Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/yashwanth461/kube.git'
            }
        }
        
       

        stage('Kubernetes Deploy') {
            steps {
                script {
                    sh '''
                    kubectl config use-context minikube
                    kubectl apply -f deploy.yml --validate=false
                    '''
                }
            }
        }
    }
}
