pipeline {
    agent any
    environment {
        IMAGE_NAME = "king094/akshatnewimg6july:v1"
        CONTAINER_NAME = "My-first-containe2211"
        HOST_PORT = "8089"
        REMOTE_USER = "ubuntu"
        REMOTE_IP = "10.0.13.189" // <-- CHANGE if needed
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

        stage('Deploy on Remote Host') {
            steps {
                script {
                    def remoteRemoveCmd = "docker rm -f ${CONTAINER_NAME} || true"
                    def remoteRunCmd = "docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:80 ${IMAGE_NAME}"

                    sshagent(['sshkeypair']) {
                        // SSH into the remote host and deploy
                        sh "ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_IP} '${remoteRemoveCmd}'"
                        sh "ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_IP} '${remoteRunCmd}'"
                    }
                }
            }
        }
    }
}
