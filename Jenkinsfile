pipeline {
    agent any

    environment {
        REMOTE_USER = 'user' // Replace with your remote username
        REMOTE_HOST = 'your-server' // Replace with your server address
        REMOTE_PATH = '/etc/nginx/nginx.conf'
        SSH_KEY_ID = 'your-ssh-key-id' // Jenkins credentials ID for SSH key
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Repository cloned.' // Implicit when using "Pipeline script from SCM"
            }
        }

        stage('Deploy Nginx Config') {
            steps {
                echo 'Starting deployment of Nginx configuration...'

                withCredentials([sshUserPrivateKey(credentialsId: "${SSH_KEY_ID}", keyFileVariable: 'KEY')]) {
                    // Copy config to remote server
                    sh '''
                        echo "Copying nginx.conf to remote server..."
                        scp -i $KEY -o StrictHostKeyChecking=no nginx.conf ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH} || exit 1
                    '''

                    // Restart Nginx on remote server
                    sh '''
                        echo "Restarting Nginx service on remote server..."
                        ssh -i $KEY -o StrictHostKeyChecking=no ${REMOTE_USER}@${REMOTE_HOST} "sudo systemctl restart nginx || exit 1"
                    '''
                }

                echo 'Nginx configuration deployed and service restarted.'
            }
        }
    }

    post {
        success {
            echo '✅ Deployment completed successfully.'
        }
        failure {
            echo '❌ Deployment failed. Please check the logs for details.'
        }
    }
}
