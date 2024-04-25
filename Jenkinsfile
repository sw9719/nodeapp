pipeline {
    agent any

   environment { // getting stored credentials
       DOCKERHUB_CREDENTIALS = credentials('imagerepo')
   }

   /*stages { // to clone repo. Enable this section if you are using inline jenkins script
       stage('SCM Checkout') {
           steps {
               git branch: 'master', url: 'https://github.com/sw9719/nodeapp.git'
           //sh 'whoami'
           }
       }
    */

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
    }

    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }
    }
}
