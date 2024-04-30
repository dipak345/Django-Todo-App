pipeline {
    agent any

    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/dipak345/Django-Todo-App.git", branch: "main"
            }
        }

        stage("Build") {
            steps {
                echo "Building the Docker image"
                sh "docker build -t todo-list-app ."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the Docker image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "poll", passwordVariable: "pwd", usernameVariable: "uname")]) {
                    sh "docker tag todo-list-app ${env.uname}/todo-list-app:latest"
                    sh "docker login -u ${env.uname} -p ${env.pwd}"
                    sh "docker push ${env.uname}/todo-list-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
