pipeline {
    agent any

    environment {
        // Defines the SSH target, which can be an IP address or hostname.
        EC2_HOST = 'ec2-user@your-ec2-public-ip'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/nginx-deploy.git'
            }
        }

        stage('Deploy NGINX') {
            steps {
                // The `sshagent` step injects the private key for the duration of this block.
                sshagent (credentials: ['your-ssh-key-id']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${EC2_HOST} << EOF
                        sudo yum update -y
                        sudo amazon-linux-extras install nginx1 -y
                        sudo systemctl start nginx
                        sudo systemctl enable nginx
                        EOF
                    """
                }
            }
        }
    }

    post {
        success {
            echo '✅ NGINX deployed successfully!'
        }
        failure {
            echo '❌ Deployment failed.'
        }
    }
}
