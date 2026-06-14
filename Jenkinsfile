@Library("Shared") _
pipeline{
    agent {label "aryan"}
    
    stages{
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage("Code"){
            steps{
                script{
                    clone("https://github.com/mebidhanshrestha/django-notes-app.git","main")
                }
            }
        }
        stage("Build"){
            steps{
                echo "Building with docker compose"
                sh "docker compose build"
            }
        }
        stage("Test"){
            steps{
                echo "This is testing the code"
            }
        }
        stage("Push to DockerHub"){
            steps{
                echo "This is pushing the image to Docker Hub"
                withCredentials([usernamePassword('credentialsId':"dockerHubCred",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${dockerHubPass}"
                    sh "docker image tag django_app:latest ${env.dockerHubUser}/django_ap:latest"
                    sh "docker push ${env.dockerHubUser}/django_ap"
                }
            }
        }
        stage("Deploy"){
            steps{
                echo "Deploying with docker compose"
                sh "docker compose down || true"
                sh "docker compose up -d"
            }
        }
    }
}
