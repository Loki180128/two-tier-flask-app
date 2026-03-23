pipeline{
    agent { label "dev"};
    stages{
        stage("code Clone"){
            steps{
               git url:"https://github.com/Loki180128/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
               
            }
        }
        stage("push to docher hub"){
             steps{
                 withCredentials([usernamePassword(
                     credentialsId: "dockerHubCreds",
                     passwordVariable: "dockerHubPass",
                     usernameVariable: "dockerHubUser"
                     )]){
                 sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                 sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
                 sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                 }
             }
        }
        stage("Deploy"){
             steps{
                 sh "docker compose up -d --build flask-app"
             }
        }
        
    }

post{
    sucess{
        emailext(
        subject: "Build Successful",
        body: "good News: Your Build was successful",
        to: 'jadhavaniket410@gmail.com'
        
            )
    }
    failure{
        emailext(
        subject: "Build failed",
        body: "Bad News: Your Build failed",
        to: 'jadhavaniket410@gmail.com'
            )
    }
}
    
}
