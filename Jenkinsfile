pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'pull'
                sh "ls -l"
                sh "docker build -t jenkins ."
            }
        }
        stage('Push') {
            steps {
                echo 'docker push to ECR'
                sh "docker tag jenkins 930650205391.dkr.ecr.us-east-1.amazonaws.com/jenkins"
                sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 930650205391.dkr.ecr.us-east-1.amazonaws.com"
                sh "docker push 930650205391.dkr.ecr.us-east-1.amazonaws.com/jenkins"

            }
        }
    }
}