@Library("Shared") _
pipeline{
    
    agent { label "dev"};

        
    stages{

        stage("Clean Up")
        {
            steps{
                cleanWs()
            }
        }
        
        stage("Code Clone"){
            steps{
               script{
                   clone("https://github.com/myasir14/two-tier-flask-app.git", "master")
               }
            }
        }
        stage("Trivy File System Scan"){
            steps{
                script{
                    trivy_fs()
                }
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
            
        }
        stage("Push to Docker Hub"){
            steps{
                script{
                    docker_push("dockerhubscredts","two-tier-flask-app")
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
            script{
                emailext from: 'myaaaasir@gmail.com',
                to: 'myaaaasir@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'myaaaasir@gmail.com',
                to: 'myaaaasir@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
