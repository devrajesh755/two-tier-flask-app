@Library('Shared_Libraries') _
pipeline {
    agent {label 'dev'};

    stages {
        stage("Code Cloning") {
            steps {
            script{
                clone("https://github.com/devrajesh755/two-tier-flask-app.git","main")
                }
            }
        }
        stage(" Trivy File System Scan"){
            steps{
                script{
                  trivy_fs()
                }
            }
        }
        stage("Build Process") {
            steps {
                script{
                  Build Process()
                }
            }
        }

        stage("Testing Phase") {
            steps {
                script{
                     Testing_Phase()
                }
            }
        }
        stage("Push To Docker Hub"){
            steps{
                script{
                    Push_DockerHub("dockerHubCreds","two-tier-flask-app")
                }
            }
        }

        stage("Deployment Phase") {
            steps {
                script{
                    Deployment_Phase(flask-app) 
                }  
            }
        }
    }
    post {
    success {
        emailext(
            to: "rajeshmanik721211@gmail.com",
            subject: "Build Successful",
            body: "Good News: Your build was successful.",
            attachmentsPattern: '**/result.json'
        )
    }

    failure {
        emailext(
            to: "rajeshmanik721211@gmail.com",
            subject: "Build Failed",
            body: "🚫 Bad News: Build failed! Please check the logs.",
            attachmentsPattern: '**/result.json'
        )
    }
}
}
