pipeline {
    agent any
    environment {
        //REPO_NAME = """${JOB_BASE_NAME}.toLowerCase()"""
        REPO_NAME = """jenkins"""
    }
    stages {
        stage('Build') {
            steps {
                echo 'pull'
                sh "ls -l"
                sh "whoami"
                sh "pwd"
                sh "printenv"
                sh "sudo systemctl status docker.socket"
                sh "sudo docker build -t ${REPO_NAME}:${GIT_COMMIT} ."
            }
        }
        stage('Push') {
            steps {
                echo 'sudo docker push to ECR'
                REPO_URI=$(aws ecr describe-repositories --repository-names "${REPO_NAME}" --query "repositories[0].repositoryUri" --output text 2>/dev/null || aws ecr create-repository --repository-name "${REPO_NAME}"  --query "repository.repositoryUri" --output text)
                // sh "if ! ${aws ecr describe-repositories --repository-name ${REPO_NAME}}; then aws ecr create-repository --repository-name ${REPO_NAME};fi"
                sh "sudo docker tag ${REPO_NAME}:${GIT_COMMIT} 930650205391.dkr.ecr.us-east-1.amazonaws.com/${REPO_NAME}:${GIT_COMMIT}"
                sh "sudo docker tag ${REPO_NAME}:${GIT_COMMIT} 930650205391.dkr.ecr.us-east-1.amazonaws.com/${REPO_NAME}:${BUILD_NUMBER}"
                sh "aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 930650205391.dkr.ecr.us-east-1.amazonaws.com"
                sh "sudo docker push 930650205391.dkr.ecr.us-east-1.amazonaws.com/${REPO_NAME}:${GIT_COMMIT}"
                sh "sudo docker push 930650205391.dkr.ecr.us-east-1.amazonaws.com/${REPO_NAME}:${BUILD_NUMBER}"

            }
        }
    }
}