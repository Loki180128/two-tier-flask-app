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
                 sh "docker login -u ${dockerHubUser} -p ${dockerHubPass}"
                 sh "docker image tag two-tier-flask-app ${dockerHubUser}/two-tier-flask-app"
                 sh "docker push ${dockerHubUser}/two-tier-flask-app:latest"
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
    success{
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
