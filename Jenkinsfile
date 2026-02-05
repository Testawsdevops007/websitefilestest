pipeline {
    agent any

    environment {
        // These are the IDs you created in Jenkins
        AWS_ACCESS_KEY_ID     = credentials('access-keys')
        AWS_SECRET_ACCESS_KEY = credentials('secret-access')
        
        // Configuration
        AWS_DEFAULT_REGION    = 'us-east-1' 
        S3_DESTINATION        = "s3://testwebsitefiles/tenant-a/websitefiles/"
    }

    stages {
        stage('Checkout') {
            steps {
                // 'scm' tells Jenkins to use the repo already linked in the Job settings
                checkout scm
            }
        }

        stage('Verify & List') {
            steps {
                echo "Testing AWS connection..."
                // Basic check to see if we can at least talk to S3
                sh "aws s3 ls"
            }
        }

        stage('Upload') {
            steps {
                echo "Uploading files to ${S3_DESTINATION}..."
                // Simple recursive copy (cp) is safer for troubleshooting than 'sync'
                sh "aws s3 cp . ${S3_DESTINATION} --recursive --exclude '.git/*' --exclude 'Jenkinsfile'"
            }
        }
    }

    post {
        success {
            echo "✅ Deployment finished successfully!"
        }
        failure {
            echo "❌ Deployment failed. Check the logs for 'Explicit Deny' errors."
        }
    }
}
