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
                    sh "docker login -u ${docker_user} -p ${docker_password}"
                }
                sh "docker push baggipawan/react-app:${currentBuild.number}"
            }
        }
        stage("Dev Deploy"){
            steps{
                sshagent(['docker-dev']) {
        
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.4.28 docker rm -f react 2>/dev/null "
                    sh " ubuntu@172.31.4.28 docker run -d -p 80:80 --name=react baggipawan/react-app:${currentBuild.number}"
                }
            }
        }
    }
}
