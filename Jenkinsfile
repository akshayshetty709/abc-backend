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

            // Step 1: Copy cloned repo to EC2
            sh '''
            scp -o StrictHostKeyChecking=no -r ${WORKSPACE}/. ubuntu@${EC2_IP}:/home/ubuntu/
            '''

            // Step 2: SSH into EC2 and build
            sh '''
            ssh -o StrictHostKeyChecking=no ubuntu@${EC2_IP} '
            set -e
            cd /home/ubuntu/

            docker stop bkapp || true
            docker rm bkapp || true
            docker rmi bkimage || true

            docker build -t bkimage .
            docker run -d -p 5000:5000 --name bkapp bkimage
            '
            '''
        }
    }
  }
    }
}
