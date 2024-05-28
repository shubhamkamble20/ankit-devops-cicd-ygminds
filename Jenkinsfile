pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                git 'https://github.com/shubhamkamble20/ankit-devops-cicd-ygminds.git'
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'sudo docker buildx build -t shubhamkamble408/batch-23 .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'sudo docker login -u shubhamkamble408 -p ${dockerhubpwd}'

}
                   sh 'sudo docker push shubhamkamble408/batch-23'
                }
            }
        }
        stage('EKS and Kubectl configuration'){
            steps{
                script{
                    sh 'aws eks update-kubeconfig --region us-east-1 --name ankit-cluster'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    sh 'kubectl apply -f deploymentservice.yaml'
                }
            }
        }
    }
}
