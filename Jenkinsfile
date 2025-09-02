pipeline {
    agent any

    stages {
        stage('Test Checkout') {
            steps {
                echo ' Successfully checked out the code!'
            }
        }

        stage('Deploy NGINX to EC2') {
            steps {
                echo ':rocket: Deploying NGINX to EC2 instance (13.203.114.157)'
                sshagent(['ec2']) {  credential ID
                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@13.203.114.157 << 'EOF'
                            echo ":arrows_counterclockwise: Updating package list..."
                            sudo apt-get update -y

                            echo ":package: Installing NGINX..."
                            sudo apt-get install -y nginx

                            echo ":rocket: Starting NGINX service..."
                            sudo systemctl enable nginx
                            sudo systemctl start nginx

                            echo "NGINX deployed and running on EC2!"
                        EOF
                    '''
                }
            }
        }
    }

    post {
        success {
            echo ' Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
