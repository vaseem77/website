pipeline {
    agent any

    environment {
        IMAGE_NAME = "abode-website"
    }

    stages {

        stage('Job1: Build') {
            steps {
                echo '=== BUILDING DOCKER IMAGE ==='
                sh 'docker build -t ${IMAGE_NAME}:latest .'
            }
        }

        stage('Job2: Test') {
            steps {
                echo '=== TESTING APPLICATION ==='
                sh '''
                    docker rm -f test-container 2>/dev/null || true
                    docker run -d --name test-container -p 8090:80 ${IMAGE_NAME}:latest
                    sleep 10
                    curl -f http://localhost:8090 && echo "TEST PASSED" || (echo "TEST FAILED" && exit 1)
                    docker rm -f test-container
                '''
            }
        }

        stage('Job3: Prod') {
            when {
                branch 'master'
            }
            steps {
                echo '=== DEPLOYING TO PRODUCTION ==='
                sh '''
                    docker rm -f prod-container 2>/dev/null || true
                    docker run -d --name prod-container -p 8181:80 ${IMAGE_NAME}:latest
                    echo "PRODUCTION DEPLOYED"
                '''
            }
        }

    }

    post {
        success { echo 'PIPELINE SUCCESS' }
        failure { echo 'PIPELINE FAILED' }
    }
}
