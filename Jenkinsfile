pipeline {
    agent any
    stages {
        stage('Test Checkout') {
            steps {
                echo 'Successfully checked out the code!'
            }
        }
        stage('Run Test Script') {
            steps {
                echo ' Running dummy test...'
                sh 'echo "Hello from Jenkins!"'
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
