pipeline {
    agent any
    environment {
       registry = "toby4all/tobby_pipeline"  // The name of your user and repository (which can be created manually)
       DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages {
        stage('clone Repo') {
            steps {
               git([url: 'https://github.com/toby4all/docker_myhubImage', branch: 'main'])
            }
        }
        stage('Build') {
            steps {
               bat 'docker build -t toby4all/tobby_pipeline .'
           }
        }
        stage('Login') {
           steps {
              bat 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
           }
        }
        stage('Push') {
            steps {
               bat 'docker push toby4all/tobby_pipeline'
            }
        }
    }
    post {
        always {
           bat "docker rmi ${registry}:${BUILD_NUMBER}" // Delete the local image at the end
        }
    }
}

