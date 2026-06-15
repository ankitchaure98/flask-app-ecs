pipeline{
    agent any;
    
    stages{
        stage("Clone"){
            steps{
                git url: "https://github.com/ankitchaure98/flask-app-ecs.git", branch: "main"
            }
        }
        stage("Build"){
            steps{
                sh "docker build --no-cache -t flask-app ."
            }
        }
        stage("Testing"){
            steps{
                echo "This is Test Stage"
            }
        }
        stage("Push To Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHubCred",passwordVariable: "dockerHubPass",usernameVariable: "dockerHubUser")]){
                echo "Image Push to DockerHub"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag flask-app:latest ${env.dockerHubUser}/flask-app:latest"
                sh "docker push ${env.dockerHubUser}/flask-app:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker stop flask-container && docker rm flask-container"
                sh "docker run -d --name flask-container -p 80:80 flask-app"
                sleep(time: 20, unit: 'SECONDS')
                sh "docker logs flask-container"
            }
        }
    }
}
