pipeline {
   agent any
   environment {
        DOCKER_HUB_REPO = "ramupy/flask-helloworld"
        CONTAINER_NAME = "flask-helloworld"
        DOCKER_HUB_CREDENTIALS = credentials('docker-cred')

   }
   stages {
        stage('Build') {
            steps {
              echo 'Building..'
              sh 'docker build -t $DOCKER_HUB_REPO:latest .'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing ...'
                sh 'docker stop $CONTAINER_NAME || true'
                sh 'docker rm $CONTAINER_NAME || true'
                sh 'docker run --name $CONTAINER_NAME $DOCKER_HUB_REPO /bin/bash -c "pytest test.py && flake8"'
            }
        }
        stage('Push') {
            steps {
                echo 'Pushing image ..'
                sh 'echo $DOCKER_HUB_CREDENTIALS_PSW | docker login -u $DOCKER_HUB_CREDENTIALS_USR --password-stdin '
                sh 'docker push $DOCKER_HUB_REPO:latest'
            }

        }
        stage('Deploy') {
            steps {
                echo 'Deploying ..'
                sh 'kubectl apply -f deployment.yml'
                sh 'kubectl apply -f service.yml'
            }
        }


   }

}