pipeline {
    agent any
    environment {
        REPO_NAME1 = "${JOB_BASE_NAME}"
        REPO_NAME = REPO_NAME1.toLowerCase()
    }
    stages {
        stage('Build') {
            steps {
                println(REPO_NAME);
                echo '${REPO_NAME}' 
                echo 'pull ${REPO_NAME}'
                sh "ls -l"
                sh "whoami"
                sh "pwd"
                sh "printenv"
                sh "sudo systemctl status docker.socket"
                sh "sudo docker build -t ${REPO_NAME}:${GIT_COMMIT} ."
            }
        }
        stage('REPO')  {
            steps {
                catchError(buildResult: 'SUCCESS')  {
                    sh "aws ecr describe-repositories --repository-name ${REPO_NAME}"
                }
                catchError(buildResult: 'SUCCESS')  {
                    sh "aws ecr create-repository --repository-name ${REPO_NAME}"
                }
            }
        }
        stage('Push') {
            steps {
                echo 'sudo docker push to ECR'
                sh "sudo docker tag ${REPO_NAME}:${GIT_COMMIT} 930650205391.dkr.ecr.us-east-1.amazonaws.com/${REPO_NAME}:${GIT_COMMIT}"
                sh "sudo docker tag ${REPO_NAME}:${GIT_COMMIT} 930650205391.dkr.ecr.us-east-1.amazonaws.com/${REPO_NAME}:${BUILD_NUMBER}"
                sh "aws ecr get-login-password --region us-east-1 | sudo docker login --username AWS --password-stdin 930650205391.dkr.ecr.us-east-1.amazonaws.com"
                sh "sudo docker push 930650205391.dkr.ecr.us-east-1.amazonaws.com/${REPO_NAME}:${GIT_COMMIT}"
                sh "sudo docker push 930650205391.dkr.ecr.us-east-1.amazonaws.com/${REPO_NAME}:${BUILD_NUMBER}"
            }
        }
    }
}