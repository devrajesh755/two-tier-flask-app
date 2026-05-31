pipeline {
    agent {label 'dev'};

    stages {
        stage("Code Cloning") {
            steps {
                echo "Cloning Source Code"
                git url: "https://github.com/devrajesh755/two-tier-flask-app.git",branch: "main"
                echo "Cloning Source Code Completed"
            }
        }
        stage(" Trivy File System Scan"){
            steps{
                echo "Performing File System Scan..."
                sh "trivy fs . -o result.json"
                echo "File System Scanning Completed Successfully"
            }
        }
        stage("Build Process") {
            steps {
                echo "Building..."
                sh "docker build -t two-tier-flask_app ."
                echo "Building Completed..."
            }
        }

        stage("Testing Phase") {
            steps {
                echo "Testing Code"
            }
        }
        stage("Push To Docker Hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser"
                    )]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker image tag two-tier-flask_app ${env.dockerHubUser}/two-tier-flask_app"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask_app"
                }
            }
        }

        stage("Deployment Phase") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    post {
    success {
        emailext(
            to: "rajeshmanik721211@gmail.com"
            subject: "Build Successful",
            body: "✅ Good News: Your build was successful.",
            attachmentsPattern: '**/result.json'
        )
    }

    failure {
        emailext(
            to: "rajeshmanik721211@gmail.com"
            subject: "Build Failed",
            body: "🚫 Bad News: Build failed! Please check the logs.",
            attachmentsPattern: '**/result.json'
        )
    }
}
}
