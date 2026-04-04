pipeline {
    agent {
        dockerfile true
    }
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Publish') {
            when {
                branch 'master'
            }
            steps {
                echo 'Publishing on port 82...'
                sh 'service apache2 start'
            }
        }
    }
}
