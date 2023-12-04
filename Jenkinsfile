pipeline{

    agent any

    environment{

        DOCKERHUB_USERNAME = "singhrohit444"
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
                    url: 'https://github.com/singhrohit444/gitops_argocd_project.git',
                    branch: 'main'
                }
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    docker_image = docker.build "${IMAGE_NAME}"
                }
            }
        }
        stage('Push docker image'){
            steps{
                script{
                    docker.withRegistry('',REGISTRY_CREDS){
                        docker_image.push("$BUILD_NUMBER")
                        docker_image.push('latest')
                    }
                }
            }
        }
        stage('Delete docker images from jenkins server'){
            steps{
                script{
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
                }
            }
        }
        stage('Update K8s Deployment file'){
            steps{
                script{
                    sh """
                    cat Deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' Deployment.yaml
                    cat Deployment.yaml
                    """
                }
            }
        }
        stage('Push changes to github'){
            steps{
                script{
                    sh """
                    git config --global user.name "singhrohit444"
                    git config --global user.email "singhrohit444@gmail.com"
                    git add Deployment.yaml
                    git commit -m "uploading updated deployment k8s manifest file to github"
                    """

                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    
                    sh "git push https://github.com/singhrohit444/gitops_argocd_project.git main"
                    }
                }
            }
        }
    }
}