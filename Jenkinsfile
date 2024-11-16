pipeline{
    agent { label 'dev-server' }
    
    stages{
        stage("Code Clone"){
            steps{
                echo "Code Clone Stage"
                git url: "https://github.com/LondheShubham153/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Code Build & Test"){
            steps{
                echo "Code Build Stage"
                sh "docker build -t node-app ."
            }
        }
        stage("Push to DockerHub") {
    steps {
        withCredentials([usernamePassword(
            credentialsId: "dockerHubcred", // Correct parameter name: credentialsId, not credentialsID
            usernameVariable: "dockerHubUser",
            passwordVariable: "dockerHubPass")]) {
                
            // Log in to DockerHub
            sh 'echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin'
            
            // Tag the image for DockerHub
            sh "docker image tag node-app:latest ${dockerHubUser}/node-app:latest"
            
            // Push the image to DockerHub
            sh "docker push ${dockerHubUser}/node-app:latest"
        }
    }
}

        stage("Deploy"){
            steps{
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
