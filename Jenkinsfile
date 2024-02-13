pipeline{
    agent any
    
    stages{
        
        stage("Code"){
            steps{
              echo "Clonning the code"  
              git url: "https://github.com/karansingh2004/tws-ci-cd-project.git" , branch : "main"
            }
        }
        
        stage("Build"){
            steps{
                echo "Building the image"
                sh "docker build -t notes-app ."
            }
        }
        
        stage("Push to docker"){
            steps{
                echo "Pushing image to dockerhub"
                withCredentials([usernamePassword(credentialsId: "dockerhub" ,passwordVariable: "dockerhubPass" ,usernameVariable: "dockerhubUser")]){
                    sh "docker tag notes-app ${env.dockerhubUser}/notes-app:v1"
                    sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                    sh "docker push ${env.dockerhubUser}/notes-app:v1"
                }
                
            }
        }
        
        stage("Deploy"){
            steps{
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
                // sh "docker run -d -p 8000:8000 karansingh2004/notes-app:v1"
            }
        }
    }
}
