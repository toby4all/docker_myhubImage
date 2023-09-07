pipeline {

    environment {
        registry = "toby4all/tobby_pipeline"  // The name of your user and repository (which can be created manually)
        registryCredential =credentials('docker_hub') // The credentials used for your Docker Hub repository
        dockerImage = "" // Will be overridden later
    }
    stages {
        stage('clone Repo') {
            steps {
               git([url: 'https://github.com/toby4all/docker_myhubImage', branch: 'main'])
            }
        }
        stage('Build and push image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}:${BUILD_NUMBER}") // Give a name and version to the image
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push() // Push the image to Docker Hub
                    }
                }
            }
            post {
                always {
                    script {
                        bat "docker rmi ${registry}:${BUILD_NUMBER}" // Delete the local image at the end
                    }
                }
            }
        }
    }
}

