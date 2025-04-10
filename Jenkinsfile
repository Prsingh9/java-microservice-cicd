pipeline {
    agent any

    environment {
        IMAGE_NAME = "prabsin/myapp"
        K8S= credentials('config')
    }

    stages {
        stage('Build & Test') {
    steps {
        dir('myapp') {
            sh 'mvn clean package'
        }
    }
}


        stage('Docker Build & Push') {
            when {
                branch 'develop'
            }
            steps {
                script {
                    docker.withRegistry('', 'docker-hub-credentials') {
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
