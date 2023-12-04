pipeline{

    agent any

    environment{

        DOCKERHUB_USERNAME = "singhrohit444@gmail.com"
        APP_NAME = "gitops-argo-app"
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMAGE_NAME = "${DOCKERHUB_USERNAME}" + "/" + "${APP_NAME}"
        REGISTRY_CREDS = 'dockerhub'
    }

    stages{

        stage('Clean workspace'){
            steps{
                script{
                    cleanWs()
            }
          }
        }
        stage('Checkout SCM'){
            steps{
                script{
                    git credentialsId: 'github',
                    url: 'https://github.com/singhrohit444/gitops-argocd-project.git',
                    branch: 'main'
                }
            }
        }
    }
}