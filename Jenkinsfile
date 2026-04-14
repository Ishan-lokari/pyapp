pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/Ishan-lokari/pyapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("hello-python")
                }
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run hello-python'
            }
        }
    }
}
