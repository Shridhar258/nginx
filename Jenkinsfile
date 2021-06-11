pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'pull'
                sh "ls -l"
                docker.build('jenkins')
            }
        }
        stage('Push') {
            steps {
                echo 'docker push to ECR'
                docker.withRegistry('https://1234567890.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:jenkins-ecr-credentials')
                {
                    docker.image('jenkins').push('latest')
                }

            }
        }
    }
}