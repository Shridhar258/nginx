pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'pull'
                sh "ls -l"
                sh "whoami"
                sh "pwd"
                sh "printenv"
                sh "sudo systemctl status docker.socket"
                sh "docker build -t jenkins:${GIT_COMMIT} ."
            }
        }
        stage('Push') {
            steps {
                echo 'docker push to ECR'
                sh "docker tag jenkins:${GIT_COMMIT} 930650205391.dkr.ecr.us-east-1.amazonaws.com/jenkins:${GIT_COMMIT}"
                sh "docker tag jenkins:${GIT_COMMIT} 930650205391.dkr.ecr.us-east-1.amazonaws.com/jenkins:${BUILD_NUMBER}"
                sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 930650205391.dkr.ecr.us-east-1.amazonaws.com"
                sh "docker push 930650205391.dkr.ecr.us-east-1.amazonaws.com/jenkins:${GIT_COMMIT}"
                sh "docker push 930650205391.dkr.ecr.us-east-1.amazonaws.com/jenkins:${BUILD_NUMBER}"

            }
        }
    }
}