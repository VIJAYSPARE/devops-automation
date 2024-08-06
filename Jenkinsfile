pipeline {
    agent any
    tools{
        maven 'maven_3.5.0'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/VIJAYSPARE/devops-automation.git']])
                sh 'mvn clean install'
            }
        }
            stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t vijaypare/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
    

                   
                   sh 'docker login -u vijaypare -p ${dockerhubpwd}'

}
                   sh 'docker push vijaypare/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'k8sconfigpwdnew')
                    
                }
            }
        }
    }
}
