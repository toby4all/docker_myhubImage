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
               withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubPassword', usernameVariable: 'dockerhubUser')]) {
        	       bat "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPassword}"
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

