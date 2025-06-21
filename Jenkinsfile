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
                    sh 'docker build -t dkubernetes/akshatnewimg6july:v1 .'
                    sh 'docker images'
                }
            }
        }
        stage('Push to dockerHUb'){
            steps{
                 withCredentials([usernamePassword(credentialsId: 'dockerHubCred', usernameVariable: 'dockerHubUser', passwordVariable: 'dockerHubPass')]) {
                    sh "docker login -u ${env.dockerHubUser}, -p {env.dockerHubPass} "
                    sh"docker image tag dkubernets:v1 dkubernetes/akshatnewimg6july:v1"
                    sh 'docker push dkubernetes/dkubernets:v1'
                     
                }
            }
        }
    }
}
