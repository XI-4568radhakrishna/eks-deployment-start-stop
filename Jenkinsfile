pipeline {
    agent any
    stages {
        stage('Install kubectl') {
            steps {
                script {
                    sh '''
                    curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                    chmod +x kubectl
                    sudo mv kubectl /usr/local/bin/
                    kubectl version --client
                    '''
                }
            }
        }
		stage('Set up Kubeconfig for EKS') {
            steps {
                script {
                    
                    sh "aws eks update-kubeconfig --region us-east-1 --name sdlc-eks-cluster"

                    
                }
            }
       }
		stage('Stop Pods') {
            steps {
                script {
                    sh '''
                    export KUBECONFIG=$KUBECONFIG
                    kubectl scale deployment eksdemo-fastapi --replicas=0 -n default
                    '''
                }
            }
        }
        stage('Wait Time') { // Optional
            steps {
                script {
                    sleep 60 // Adjust as needed
                }
            }
        }
        stage('Start Pods') {
            steps {
                script {
                    sh '''
                    export KUBECONFIG=$KUBECONFIG
                    kubectl scale deployment eksdemo-fastapi --replicas=3 -n default
                    '''
                }
            }
        }
    }
}
