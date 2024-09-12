pipeline{
    agent any
    
    stages{
        
        stage("Code"){
            steps{
                echo("Cloning the code")
                git url: "https://github.com/meVipulSingh/django-notes-app.git", branch: "main"
            }
        }
        stage("build"){
            steps{
                echo("Bulding the docker image")
                sh "docker build . -t mevipulsingh/notes-app"
            }
        }
        stage("Push to docker"){
            steps{
                echo("Pushing image to docker hub")
                withCredentials(
                    [usernamePassword(
                        credentialsId: "dockerHub",
                        passwordVariable: "dockerHubPass",
                        usernameVariable: "dockerHubUser"
                        )
                    ]
                ){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/notes-app"
                    
                }
            }
        }
        stage("Deploy"){
            steps{
                echo("Deploying the application")
                sh "docker compose up -d"
            }
        }
    }
}
