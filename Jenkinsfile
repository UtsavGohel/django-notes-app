pipeline {
    agent any
    
    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the app"
                git url:"https://github.com/UtsavGohel/django-notes-app.git",branch:"master"
            }
        }
        stage("Build") {
            steps {
                echo "Building the code"
                sh "docker build -t notes-app ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag notes-app ${env.dockerHubUser}/notes-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the container"
                // sh "docker run -d -p 8000:8000 ustavgohel/notes-app:latest"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
