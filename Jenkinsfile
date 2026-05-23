pipeline {
    agent any
    stages {
        stage('Job1: Build') {
            steps {
                echo "JOB 1 : BUILD"
                sh 'docker build -t abodesoftware:latest .'
            }
        }
        stage('Job2: Test') {
            steps {
                sh 'docker rm -f test_container || true'
                sh 'docker run -d --name test_container -p 8181:80 abodesoftware:latest'
                sh 'sleep 5'
                sh 'curl -f http://localhost:8181 && echo TEST PASSED || echo TEST FAILED'
            }
            post {
                always { sh 'docker rm -f test_container || true' }
            }
        }
        stage('Job3: Prod') {
            when { branch 'master' }
            steps {
                sh 'docker rm -f prod_container || true'
                sh 'docker run -d --name prod-container -p 8181:80 ${IMAGE_NAME}:latest'
                echo "Deployed to prod"
            }
        }
    }
}
