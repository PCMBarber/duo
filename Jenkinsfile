pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t stratcastor/duo-deploy-flask:latest -t stratcastor/duo-deploy-flask:v$BUILD_NUMBER .
                '''
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push stratcastor/duo-deploy-flask:latest
                docker push stratcastor/duo-deploy-flask:v$BUILD_NUMBER
                '''
            }
        }
        stage('Cleanup') {
            steps {
                sh '''
                docker rmi stratcastor/duo-deploy-flask:latest
                docker rmi stratcastor/duo-deploy-flask:v$BUILD_NUMBER
                '''
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./k8s-deployments -n production
                kubectl rollout restart deployment flask-deployment -n production
                '''
            }
        }
    }
}