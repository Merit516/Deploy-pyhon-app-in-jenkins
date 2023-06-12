pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[
                        url: 'https://github.com/Merit516/Python-App.git',
                        credentialsId: 'GitHub'
                    ]]
                ])
            }
        }
        stage('Build') {
            steps {
                git 'https://github.com/Merit516/Python-App'
                withCredentials([usernamePassword(credentialsId: 'Dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD_iti')]) {
                    sh '''
                        ls
                          docker login -u ${USERNAME} -p ${PASSWORD_iti}
                          docker build .  -f dockerfile -t merit237/python-app:v5 --network host
                          docker push merit237/python-app:v5
                    '''
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
