pipeline {
    agent any
    environment {
        registry = "toby4all/tobby_pipeline"
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages {
        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/toby4all/docker_myhubImage', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                bat 'docker build -t toby4all/tobby_pipeline:latest .'
            }
        }
        stage('Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'dockerhubPassword', usernameVariable: 'dockerhubUser')]) {
                    bat "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPassword}"
                }
            }
        }
        stage('Push') {
            steps {
                bat 'docker push toby4all/tobby_pipeline:latest'
            }
        }
    }
    post {
        always {
            bat "docker rmi ${registry}:latest"
        }
    }
}
