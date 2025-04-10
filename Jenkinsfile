pipeline {
    agent any

    environment {
        IMAGE_NAME = "prabsin/myapp"
    }

    stages {
        stage('Build & Test') {
            steps {
                sh 'cd myapps'
                sh 'mvn clean package'
            }
        }

        stage('Docker Build & Push') {
            when {
                branch 'develop'
            }
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials-id') {
                        def image = docker.build("${IMAGE_NAME}:${env.BUILD_NUMBER}")
                        image.push()
                        image.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            when {
                branch 'develop'
            }
            steps {
                sh '''
                kubectl apply -f k8s/deployment.yaml
                kubectl apply -f k8s/service.yaml
                '''
            }
        }
    }
}
