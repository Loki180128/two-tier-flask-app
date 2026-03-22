@Library("my-shared-library") _  // 'my-shared-library' is the name you set in Jenkins Global Settings

pipeline {
    agent any

    stages {
        stage("Code Clone") {
            steps {
                script {
                    // This calls 'vars/clone.groovy' from your library
                    clone("https://github.com/Loki180128/two-tier-flask-app.git", "master")
                }
            }
        }

        stage("Security Scan") {
            steps {
                script {
                    // This calls 'vars/trivy_fs.groovy' from your library
                    trivy_fs()
                }
            }
        }

        stage("Build & Push") {
            steps {
                script {
                    // This calls 'vars/docker_push.groovy' from your library
                    // It handles the login, tagging, and pushing automatically
                    docker_push("dockerHubCreds", "two-tier-flask-app")
                }
            }
        }

        stage("Deploy") {
            steps {
                // Deployment is usually specific to the app, so we keep it here
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}
