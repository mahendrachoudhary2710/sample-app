pipeline {
    agent any

    environment {
        IMAGE = "mahendrachoudhary2710/sample-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git 'https://github.com/mahendrachoudhary2710/sample-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE .'
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $IMAGE
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop app || true
                docker rm app || true
                docker run -d -p 80:3000 --name app $IMAGE
                '''
            }
        }
    }
}
