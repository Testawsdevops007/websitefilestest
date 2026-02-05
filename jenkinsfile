
pipeline {
    agent any

    environment {
        // AWS KEYS (Hardcoded for testing)
        AWS_ACCESS_KEY_ID     = 'AKIAQUFLQDGW4GGKWKFB' 
        AWS_SECRET_ACCESS_KEY = 'A+nB0YhzRdKa7P1J7jtV1VJq+jEmNn0y1k7GVj+E'
        AWS_DEFAULT_REGION    = 'us-east-1' // Change to your bucket region
        
        // REPO & BUCKET
        REPO_URL    = 'https://github.com/Testawsdevops007/websitefilestest.git'
        BUCKET_NAME = 'testwebsitefiles'
    }

    stages {
        stage('Clone GitHub Repo') {
            steps {
                // This manually clones the repo into the workspace
                git url: "${REPO_URL}", branch: 'main' 
                sh 'ls -la' // List files to confirm they arrived
            }
        }

        stage('Upload to S3') {
            steps {
                echo "Syncing files from GitHub to S3 bucket: ${BUCKET_NAME}"
                
                // The AWS CLI uses the environment variables above automatically
                // This syncs the current directory to S3, excluding git metadata
                sh "aws s3 sync . s3://${BUCKET_NAME}/ --exclude '.git/*'"
            }
        }
    }
}
