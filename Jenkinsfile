pipeline {
    agent any
    stages {
        stage("Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/Nyadav123/django-notes-app.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Build the code"
                sh "docker build -t my-notes ."
                echo "List Docker images"
                sh "docker images"
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Push the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerhubPass", usernameVariable: "dockerhubUser")]) {
                    sh "docker tag my-notes ${env.dockerhubUser}/my-notes:latest"
                    sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                    sh "docker push ${env.dockerhubUser}/my-notes:latest"
                }
            
        }
        }
        stage("Deploy") {
            steps {
                echo "Deploy the code"
                sh "docker-compose down && docker-compose up -d"
                // Add deployment steps here
            }
        }
    }
}
