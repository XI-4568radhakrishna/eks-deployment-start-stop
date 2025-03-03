pipeline {
    agent any
    stages {
        stage('Install kubectl') {
            steps {
                script {
                    sh '''
                    curl -O "https://s3.us-west-2.amazonaws.com/amazon-eks/1.32.0/2024-12-20/bin/linux/arm64/kubectl"
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
                    kubectl scale deployment my-app --replicas=0 -n my-namespace
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
                    kubectl scale deployment my-app --replicas=3 -n my-namespace
                    '''
                }
            }
        }
    }
}
