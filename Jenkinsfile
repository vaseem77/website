pipeline {
    agent any

    stages {

        stage('Build Docker Image') {
            steps {
                sh '''
                    docker build -t mywebsite:latest .
                '''
            }
        }

        stage('Publish - Master Branch') {
            when {
                branch 'master'
            }
            steps {
                sh '''
                    docker rm -f website-master || true
                    docker run -d \
                        --name website-master \
                        -p 82:80 \
                        -v $(pwd):/var/www/html \
                        mywebsite:latest
                    echo "Website published at port 82"
                '''
            }
        }

        stage('Build Only - Develop Branch') {
            when {
                branch 'develop'
            }
            steps {
                echo 'Develop branch: Image built. Not publishing.'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs.'
        }
    }
}
