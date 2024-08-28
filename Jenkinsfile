pipeline {
    agent any

    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/Buvana23/My-app.git", branch: "main"
            }
        }

        stage("Build") {
            steps {
                echo "Building the Docker image"
                sh "docker build -t My-app ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    sh "docker tag My-app ${env.dockerHubUser}/my-app:latest"
                    sh "echo ${env.dockerHubPass} | docker login -u ${env.dockerHubUser} --password-stdin"
                    sh "docker push ${env.dockerHubUser}/My-app:latest"
                }
            }
        }

        stage ("Deploy") {
            steps {
                echo "Deploying the container"
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}