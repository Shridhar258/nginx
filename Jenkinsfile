pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'pull'
                sh "ls -l"
            }
        }
        stage('Test') {
            steps {
                echo 'docker build'
                sh "sudo docker build -t ."
                sh "sudo docker images -a"
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh "sudo docker run -d nginx"
            }
        }
    }
}