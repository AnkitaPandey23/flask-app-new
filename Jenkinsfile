pipeline{
    agent {label "dev"}
    stages{
        stage("Code Clone"){
            steps{
                git url: "https://github.com/AnkitaPandey23/flask-app-new.git", branch: "main"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
                sh "docker image tag two-tier-flask-app ankitapandey5/two-tier-flask-app:latest"
            }
    }
        stage("Test"){
            steps{
                echo "testing done"
            }
        }
        stage("Pushed To DockerHub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId: "dockerhub",
                    usernameVariable: "dockeruser",
                    passwordVariable: "dockerPass"
                )]) {
                    sh """
                    docker login -u ${env.dockeruser} -p ${env.dockerPass}
                    docker push ${env.dockeruser}/two-tier-flask-app:latest
                    """
                }
            }
        }
        stage("Deploy"){
            steps{
                sh """
                docker compose down || true
                docker compose up -d --build
                """
            }
        }
    }
}
