pipeline {
    agent { label 'slave1' }

    stages {
        stage('Pull Code from GitHub') {
            steps {
                git 'https://github.com/<your-username>/<your-repo-name>.git'
            }
        }

        stage('Run Hello World') {
            steps {
                bat 'python pyapp\\hello.py'
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
