pipeline {
    agent any
    environment {
        IMAGE_NAME = "king094/akshatnewimg6july:v1"
    }
    stages {
        stage('Clone Git Repo') {
            steps {
                git url: 'https://github.com/dkubernets/php-project/', branch: 'master'
            }
        }
        stage('Build Docker Image with Buildx') {
            steps {
                script {
                    // Ensure buildx builder exists and is used
                    sh '''
                        docker buildx create --name mybuilder --use || echo "builder exists"
                        docker buildx inspect --bootstrap
                        docker buildx build -t $IMAGE_NAME . --load
                    '''
                }
            }
        }
        stage('Docker Login and Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerHubCred', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh '''
                        echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin docker.io
                        docker push $IMAGE_NAME
                    '''
                }
            }
        }
        stage('Deploy') {
            steps {
               script {
                   def dockerrm = 'sudo docker rm -f My-first-containe2211 || true'
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe2211 -p 8083:80 king094/akshatnewimg6july:v1'
                    sshagent(['sshkeypair']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe2111 -p 8083:80 dkubernetes/2febimg:v1"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@10.0.13.189 ${dockerrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@10.0.13.189 ${dockerCmd}"
                    }
                }
            }
        } 
    }
}
