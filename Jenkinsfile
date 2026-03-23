pipeline {
    agent { label "dev" } // Removed the semicolon
    stages {
        stage("Code Clone") {
            steps {
                git url: "https://github.com/Loki180128/two-tier-flask-app.git", branch: "master"
            }
        }
        stage("Trivy File System Scan"){
            steps{
                sh "trivy fs . -o results.json"
            }
        }
        stage("Build") {
            steps {
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
                    // Security tip: Use --password-stdin to avoid showing password in process logs
                    sh "echo ${dockerHubPass} | docker login -u ${dockerHubUser} --password-stdin"
                    
                    // Explicitly tag as latest to match the push command
                    sh "docker tag two-tier-flask-app ${dockerHubUser}/two-tier-flask-app:latest"
                    sh "docker push ${dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                // Note: --build here will rebuild the image locally. 
                // Since you just pushed to Hub, you could also just pull it.
                sh "docker compose up -d --build flask-app"
            }
        }
    }

    post {
        success {
            emailext(
                subject: "Build Successful",
                body: "Good News: Your Build was successful",
                to: 'jadhavaniket410@gmail.com'
            )
        }
        failure {
            emailext(
                subject: "Build failed",
                body: "Bad News: Your Build failed",
                to: 'jadhavaniket410@gmail.com'
            )
        }
    }
}
