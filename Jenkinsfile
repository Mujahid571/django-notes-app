@Library("Shared") _
pipeline{
    agent any
    
    stages{
        stage("hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        
        stage("code"){
            steps{
                echo "This is the cloning the code"
                git url: "https://github.com/Mujahid571/django-notes-app.git",branch:"main"
                echo "code clonning successfull"
            }
            
        }
        stage("build"){
             steps{
                  echo "This is the building the code"
                  sh "docker build -t notes-app:latest ."
             }
        }
        stage("push"){
             steps{
                echo  "This is pushing the code to docker hub"
                withCredentials([usernamePassword(
                    'credentialsId':"dockerHubCred",
                     passwordVariable:'dockerHubPass',
                     usernameVariable:"dockerHubUser")]){
                    
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag notes-app:latest ${env.dockerHubUser}/notes-app:v1"
                sh "docker push ${env.dockerHubUser}/notes-app:v1"
                }
             }
        }
        stage("Deploy"){
             steps{
                 echo "This is deploying the code"
                 sh "docker compose up -d"
             }
        }
    }
}
