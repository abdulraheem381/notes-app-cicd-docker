pipeline {
    agent any

    stages {

        stage("Clone") {
            steps {
                echo "Clone the code from the repo"
                // ✅ 'branch' must be key:value, not key value
                git url: "https://github.com/abdulraheem381/notes-app-cicd-docker", branch: "main"
            }
        }

        stage("Build") {
            steps {
                echo "This step will build docker image"
                sh "docker build -t notes-app:v1 ."
            }
        }

        stage("Push") {
            steps {
                echo "Push the image to docker hub!"
                withCredentials([usernamePassword(
                    credentialsId: "dockerhub",
                    usernameVariable: "dockerhubUser",   // ✅ use proper variable names
                    passwordVariable: "dockerhubPass")]) {
                    
                    sh 'echo $dockerhubPass | docker login -u $dockerhubUser --password-stdin'
                    sh "docker image tag notes-app:v1 ${dockerhubUser}/notes-app:latest"
                    sh "docker push ${dockerhubUser}/notes-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                echo "Run the container"
                sh "docker compose up -d"
            }
        }
    }
}
