pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'pull'
                sh "ls -l"
                docker.build('nginx')
            }
        }
        stage('Push') {
            steps {
                echo 'docker push to ECR'
                docker.withRegistry('https://1234567890.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:nginx-ecr-credentials')
                {
                    docker.image('nginx').push('latest')
                }

            }
        }
    }
}