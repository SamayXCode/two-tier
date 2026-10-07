pipeline{
    agent { label "dev"};
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/SamayXCode/two-tier.git", branch: "main"
            }
        }
        stage("Trivy File System Scan") {
    steps {
        sh "trivy fs --severity HIGH,CRITICAL --exit-code 1 ."
            }
        }
        stage("Build"){
            steps{
                sh "docker build -t flask-app:latest ."
            }
        }
        stage("Test"){
            steps{
                echo "dev/tester tests likh ke dega... "
            }
        }
        stage("push to docker hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    passwordVariable:"dockerHubPass",
                    usernameVariable:"dockerHubUser"
                    )]){
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        sh "docker image tag flask-app:latest ${env.dockerHubUser}/two-tier-flask-app"
                        sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                    }
            }
        }
        stage("deploy"){
            steps{
               sh "docker compose up -d --build flask-app"
            }
        }
    }
    post{
        success{
            script{
                emailext from: 'negisamay6@gmail.com',
                to: 'negisamay6@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'negisamay6@gmail.com',
                to: 'negisamay6@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
