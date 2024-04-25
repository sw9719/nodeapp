pipeline {
    agent any

       stage('Docker Login') {
           steps {
               echo 'Logon in to docker hub'
               sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin docker.io'
               echo 'Login Successfull'
           }
       }



        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("docker.io/nodeapp:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://docker.io', 'imagerepo') {
                        dockerImage.push()
                    }
                }
            }
        }

    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }
    }
}
