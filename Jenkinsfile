pipeline {
    agent any

    environment {
        // AWS Secrets (Mapping your IDs to AWS variables)
        AWS_ACCESS_KEY_ID     = credentials('access-keys')
        AWS_SECRET_ACCESS_KEY = credentials('secret-access')
        
        // AWS Configuration
        AWS_DEFAULT_REGION    = 'us-east-1' 
        S3_PATH               = "s3://testwebsitefiles/tenant-a/websitefiles/"
        
        // GitHub Configuration
        REPO_URL              = 'https://github.com/Testawsdevops007/websitefilestest.git'
        GITHUB_CREDS_ID       = 'github-token' // The ID you gave your GitHub PAT in Jenkins
    }

    stages {
        stage('Checkout Source') {
            steps {
                echo "Pulling code from GitHub..."
                // This step clones your repo into the Jenkins workspace
                git branch: 'main', 
                    credentialsId: "${GITHUB_CREDS_ID}", 
                    url: "${REPO_URL}"
            }
        }

        stage('Deploy to S3') {
            steps {
                echo "Syncing files to ${S3_PATH}..."
                
                // The AWS CLI uses the environment variables automatically
                sh "aws s3 sync . ${S3_PATH} --exclude '.git/*' --exclude 'Jenkinsfile' --delete"
            }
        }
    }

    post {
        success {
            echo "✅ Successfully deployed to S3!"
        }
        failure {
            echo "❌ Build failed. Check the logs for GitHub or AWS errors."
        }
    }
}
