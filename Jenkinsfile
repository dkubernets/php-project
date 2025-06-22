pipeline {
    agent any
    environment {
        IMAGE_NAME = "king094/akshatnewimg6july:v1"
        CONTAINER_NAME = "my-php-container"
        HOST_PORT = "8083"
        REMOTE_USER = "ubuntu"
        REMOTE_IP = "10.0.13.189" // Replace with your actual private IP
    }
    stages {
        stage('Clone Git Repo') {
            steps {
                git url: 'https://github.com/dkubernets/php-project/', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Use classic docker build to avoid buildx issues
                    sh '''
                        docker build -t $IMAGE_NAME .
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

        stage('Deploy to Remote Host via SSH') {
            steps {
                script {
                    def removeContainer = "docker rm -f ${CONTAINER_NAME} || true"
                    def runContainer = "docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:80 ${IMAGE_NAME}"
                    sshagent(['sshkeypair']) {
                        sh """
                            ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_IP} '${removeContainer}'
                            ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_IP} '${runContainer}'
                        """
                    }
                }
            }
        }
    }
}
