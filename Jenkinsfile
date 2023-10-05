pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker build -t stratcastor/duo-deploy-flask:latest -t stratcastor/duo-deploy-flask:v$BUILD_NUMBER .
                        '''
                    } else {
                        sh '''
                        echo "Build Not required"
                        '''
                    }
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker push stratcastor/duo-deploy-flask:latest
                        docker push stratcastor/duo-deploy-flask:v$BUILD_NUMBER
                        '''
                    } else {
                        sh '''
                        echo "Push Not required"
                        '''
                    }
                }
            }
        }
        stage('Cleanup') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        docker rmi stratcastor/duo-deploy-flask:latest
                        docker rmi stratcastor/duo-deploy-flask:v$BUILD_NUMBER
                        '''
                    } else {
                        sh '''
                        echo "Cleanup Not required"
                        '''
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        kubectl apply -f ./k8s-deployments -n development
                        kubectl rollout restart deployment flask-deployment -n development
                        '''
                    } else if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        kubectl apply -f ./k8s-deployments -n production
                        kubectl rollout restart deployment flask-deployment -n production
                        '''
                    } else {
                        sh '''
                        echo "Branch not recognised"
                        '''
                    }
                }
            }
        }
    }
}