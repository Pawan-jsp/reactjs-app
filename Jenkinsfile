pipeline{
    agent any
    
    stages{
        stage("Git Checkout"){
            steps{
                git url:"https://github.com/javahometech/reactjs-app",branch:"main"
            }
        }
        stage("Docker Build"){
            steps{
                sh "docker build -t baggipawan/react-app:${currentBuild.number} ."
            }
        }
        stage("Docker Push"){
            steps{
                withCredentials([string(credentialsId: 'Docker_Dev', variable: 'Docker_Dev')]) {
                    sh "docker login -u baggipawan -p ${Docker_Dev}"
                }
                sh "docker push baggipawan/react-app:${currentBuild.number}"
            }
        }
        stage("Dev Deploy"){
            steps{
                sshagent(['Docker_Server_ssh']) {
        
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@13.127.1.247 docker rm -f react 2>/dev/null "
                    sh "ssh ubuntu@13.127.1.247 docker run -d -p 80:80 --name=react baggipawan/react-app:${currentBuild.number}"
                }
            }
        }
    }
}
