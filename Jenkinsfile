pipeline {
    agent any

    environment {
        IMAGE_NAME = "ramkumarbaghel/docker-demo"   // Docker Hub repo
        IMAGE_TAG = "1.0"
        CONTAINER_NAME = "docker-demo-container"
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Pulling code from GitHub..."
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ./docker-demo"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo 'Pushing image to Docker Hub...'

                    // Already logged in to Docker Hub manually
                    // withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    //     sh "echo $PASS | docker login -u $USER --password-stdin"
                    // }

                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Pull Image (Optional)') {
            steps {
                script {
                    echo 'Pulling image from Docker Hub...'
                    sh "docker pull ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo "Stopping old container if exists..."
                    sh "docker rm -f ${CONTAINER_NAME} || true"

                    echo "Running new container..."
                    sh "docker run -d -p 3000:3000 --name ${CONTAINER_NAME} ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed."
        }
    }
}
