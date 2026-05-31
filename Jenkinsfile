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
                  Build_Process()
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
                    Deployment_Phase("flask-app") 
                }  
            }
        }
    }
    post {
    success {
       script {
            email_succ("rajeshmanik721211@gmail.com")
        }
    }

    failure {
        script{
            email_fail("rajeshmanik721211@gmail.com")
    }
}
}
