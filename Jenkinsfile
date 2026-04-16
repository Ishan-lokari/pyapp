pipeline {
    agent any

    environment {
        PROJECT_ID = "l2-ishan"
        REGION = "us-central1"
        IMAGE = "us-central1-docker.pkg.dev/l2-ishan/pyapp/hello-python"
    }

    stages {

        stage('Fetch from GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Ishan-lokari/pyapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build --no-cache -t hello-python .'
            }
        }

        stage('Auth to GCP') {
            steps {
                withCredentials([file(credentialsId: 'ishan_lokari', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                    sh '''
                    gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                    gcloud config set project $PROJECT_ID
                    gcloud auth configure-docker us-central1-docker.pkg.dev -q
                    '''
                }
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag hello-python $IMAGE'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE'
            }
        }

        stage('Deploy to Cloud Run') {
            steps {
                sh '''
                gcloud run deploy pyapp-service \
                --image $IMAGE \
                --platform managed \
                --region $REGION \
                --allow-unauthenticated
                '''
            }
        }
    }

    post {
        success {
            echo "Deployment successful!"
        }
        failure {
            echo  Deployment failed"
        }
    }
}
