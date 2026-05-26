pipeline {
    agent any

    environment {
        EC2_IP = "52.66.188.69"
    }

    stages {

        stage('clone backend repo') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-credentials',
                    url:'https://github.com/akshayshetty709/abc-backend.git'
            }
        }

        stage('install & build') {
            steps {
                sshagent(credentials: ['ec2_key']) {

                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@${EC2_IP} '
                    set -e

                    docker build -t bkimage .
                    docker run -d -p 3000:3000 --name bkapp bkimage
                    '
                    '''
                }
            }
        }
    }
}
