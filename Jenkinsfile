pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'sidratalib/jenkins-demo'
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Build') {
            steps {
                echo 'Building...'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }

        // ✅ NEW: Docker Build Stage
        stage('Docker Build') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$IMAGE_TAG .'
                sh 'docker tag $DOCKER_IMAGE:$IMAGE_TAG $DOCKER_IMAGE:latest'
            }
        }

        stage('Push to Hub') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerhub-creds',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
        )]) {
            sh '''
                echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
            '''
            sh 'docker push $DOCKER_USER/jenkins-demo:$BUILD_NUMBER'
            sh 'docker push $DOCKER_USER/jenkins-demo:latest'
        }
    }
}

        // ✅ NEW: Deploy Stage
        stage('Deploy') {
            steps {
                sh 'docker stop jenkins-demo || true'
                sh 'docker rm jenkins-demo || true'
                sh 'docker pull $DOCKER_IMAGE:latest'
                sh 'docker run -d -p 3000:3000 --name jenkins-demo $DOCKER_IMAGE:latest'
                echo 'App deployed at http://localhost:3000'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs!'
        }
    }
}
