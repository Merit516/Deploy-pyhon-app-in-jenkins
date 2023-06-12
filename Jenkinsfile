pipeline {
    agent any
        {
    
   
        
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                        ls
                        docker login -u ${USERNAME} -p ${PASSWORD}
                        docker build . -t merit237/python-app:v1
                        docker push merit237/python-app:v1
                    """
                } 
            }
        }
        
        stage('Deploy') {
            steps {
                sh """
                    
                    kubectl apply -f Deployment/app-ns.yaml
                    kubectl apply -f Deployment/app-deploy.yaml
                    kubectl apply -f Deployment/app-loadbalancer.yaml
                """
            }
        }
    }
}
