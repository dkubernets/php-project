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
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: "${DOCKER_REGISTRY_CREDS}", passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh "echo \$DOCKER_PASSWORD | docker login -u \$DOCKER_USERNAME --password-stdin docker.io"
                    sh 'docker push dkubernetes/akshatnewimg6july:v1'
                }
            }
        }
    }
}
