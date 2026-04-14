pipeline {
    agent any

    environment {
        PROJECT_ID = "ishan-project-489809"
        REGION = "us-central1"
        IMAGE = "us-central1-docker.pkg.dev/ishan-project-489809/pyapp/hello-python"
    }

    stages {

        stage('Checkout Repo') {
            steps {
                git 'https://github.com/Ishan-lokari/pyapp.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build --no-cache -t hello-python pyapp/'
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
            echo "🚀 pyapp deployed successfully to Cloud Run!"
        }
        failure {
            echo "❌ Deployment failed"
        }
    }
}
