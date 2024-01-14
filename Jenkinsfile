pipeline {
    agent any
    tools{
        maven 'maven'
    }
    stages{
        stage('Build Maven'){
            steps{
                git 'https://github.com/KaustubhChavan09/Terraform-cicd-project2.git'
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker buildx build -t kaustubh1805/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u kaustubh1805 -p ${dockerhubpwd}'

}
                   sh 'docker push kaustubh1805/devops-integration'
                }
            }
        }
        stage('EKS and Kubectl configuration'){
            steps{
                script{
                    sh 'aws eks update-kubeconfig --region us-east-2 --name kaustubh-cluster'
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
