pipeline {
    agent any
    stages{
        stage('git cloned'){
            steps{
                git url:'https://github.com/dkubernets/php-project/', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t dkubernets:v1'
                    sh 'docker images'
                }
            }
        }
        stage('Push to dockerHUb'){
            steps{
                 withCredentials([usernamePassword(credentialsId: 'dockerHubCred', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPass')]) {
                    sh "docker login -u ${env.dockerHubUser}, -p {env.dockerHubPass} "
                    sh"docker image tag dkubernets:v1 dkubernets/dkubernets:v1"
                    sh 'docker push dkubernets/dkubernets:v1'
                     
                }
            }
        }
    }
}
