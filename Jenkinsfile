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
                sh "sudo docker build -t jenkins:${GIT_COMMIT} ."
            }
        }
        stage('Push') {
            steps {
                echo 'sudo docker push to ECR'
                sh "sudo docker tag jenkins:${GIT_COMMIT} 930650205391.dkr.ecr.us-east-1.amazonaws.com/jenkins:${GIT_COMMIT}"
                sh "sudo docker tag jenkins:${GIT_COMMIT} 930650205391.dkr.ecr.us-east-1.amazonaws.com/jenkins:${BUILD_NUMBER}"
                sh "aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 930650205391.dkr.ecr.us-east-1.amazonaws.com"
                sh '''aws ecr describe-repositories --repository-names $1 2>&1 > /dev/null
                status=$?
                if [[ ! "${status}" -eq 0 ]]; then
                    aws ecr create-repository --repository-name $1
                fi'''
                sh "sudo docker push 930650205391.dkr.ecr.us-east-1.amazonaws.com/jenkins:${GIT_COMMIT}"
                sh "sudo docker push 930650205391.dkr.ecr.us-east-1.amazonaws.com/jenkins:${BUILD_NUMBER}"

            }
        }
    }
}