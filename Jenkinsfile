pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'pull'
                sh "ls -l"
                sh "sudo docker build -t jenkins ."
            }
        }
        stage('Push') {
            steps {
                echo 'docker push to ECR'
                sh "sudo docker tag jenkins 930650205391.dkr.ecr.region.amazonaws.com/jenkins"
                sh "sudo docker push 930650205391.dkr.ecr.region.amazonaws.com/jenkins"

            }
        }
    }
}