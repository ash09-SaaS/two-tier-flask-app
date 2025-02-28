pipeline{
    agent { label 'dev'};
    
    stages{
        stage("Code colne"){
            steps{
                git url: "https://github.com/ash09-SaaS/two-tier-flask-app.git", branch: "master"
                
            }
        }
        stage("code build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    passwordVariable: "dockerHubPass",
                    usernameVariable: "dockerHubUser"
                )]) {
                    sh "docker login -u $dockerHubUser -p $dockerHubPass"
                    sh "docker image tag two-tier-flask-app $dockerHubUser/two-tier-flask-app"
                    sh "docker push $dockerHubUser/two-tier-flask-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
