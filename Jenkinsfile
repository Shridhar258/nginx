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
                sh "sudo docker build -t ${JOB_BASE_NAME}:${GIT_COMMIT} ."
            }
        }
        stage('Push') {
            steps {
                echo 'sudo docker push to ECR'
                sh "aws ecr create-repository --repository-name ${JOB_BASE_NAME}.toLowerCase()"
                sh "sudo docker tag ${JOB_BASE_NAME}:${GIT_COMMIT} 930650205391.dkr.ecr.us-east-1.amazonaws.com/${JOB_BASE_NAME}:${GIT_COMMIT}"
                sh "sudo docker tag ${JOB_BASE_NAME}:${GIT_COMMIT} 930650205391.dkr.ecr.us-east-1.amazonaws.com/${JOB_BASE_NAME}:${BUILD_NUMBER}"
                sh "aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 930650205391.dkr.ecr.us-east-1.amazonaws.com"
                sh "sudo docker push 930650205391.dkr.ecr.us-east-1.amazonaws.com/${JOB_BASE_NAME}:${GIT_COMMIT}"
                sh "sudo docker push 930650205391.dkr.ecr.us-east-1.amazonaws.com/${JOB_BASE_NAME}:${BUILD_NUMBER}"

            }
        }
    }
}