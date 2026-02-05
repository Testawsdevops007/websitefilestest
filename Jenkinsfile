pipeline {
    agent any

    environment {
        // AWS Credentials (Secret Text IDs)
        AWS_ACCESS_KEY_ID     = credentials('access-keys')
        AWS_SECRET_ACCESS_KEY = credentials('secret-access')
        
        // AWS Configuration
        AWS_DEFAULT_REGION    = 'us-east-1' 
        S3_TARGET             = "s3://testwebsitefiles/tenant-a/websitefiles/"
        
        // GitHub Configuration
        REPO_URL              = 'https://github.com/Testawsdevops007/websitefilestest.git'
        GITHUB_CRED_ID        = 'github-creds' // The ID you gave your GitHub token in Jenkins
    }

    stages {
        stage('Checkout Repo') {
            steps {
                echo "Cloning ${REPO_URL}..."
                // Pulls your code using the Jenkins-stored GitHub credentials
                git branch: 'main', 
                    credentialsId: "${GITHUB_CRED_ID}", 
                    url: "${REPO_URL}"
            }
        }

        stage('Sync to S3') {
            steps {
                echo "Syncing index.html and files to S3..."
                
                // This command pushes the current files directly to your specific path
                // Since you fixed the 'ListAllMyBuckets' permission, sync will now work!
                sh "aws s3 sync . ${S3_TARGET} --exclude '.git/*' --exclude 'Jenkinsfile' --delete"
            }
        }
    }

    post {
        success {
            echo "✅ Successfully updated website at ${S3_TARGET}"
        }
    }
}
