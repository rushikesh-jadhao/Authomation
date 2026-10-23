
pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'ls -la'
            }
        }

        stage('Build Image') {
            steps {
                sh 'docker build -t itiss-website:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker rm -f itiss-website || true

                    docker run -d \
                      --name itiss-website \
                      --restart unless-stopped \
                      -p 8081:80 \
                      itiss-website:latest
                '''
            }
        }

        stage('Verify Website') {
            steps {
                sh 'curl --fail http://localhost:8081/'
            }
        }
    }
}
