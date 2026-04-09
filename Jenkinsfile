pipeline {
    agent { label 'slave1' }

    stages {
        stage('Pull Code from GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Ishan-lokari/pyapp.git'
            }
        }

        stage('Run Hello World') {
            steps {
                bat 'python hello.py'
            }
        }
    }

    post {
        success {
            echo 'Python Hello World ran successfully on slave1!'
        }
        failure {
            echo 'Something went wrong!'
        }
    }
}
