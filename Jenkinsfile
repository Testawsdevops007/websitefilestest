pipeline {
    agent any

    parameters {
        /* If you click "Build with Parameters", you can pick one.
           If GitHub triggers this automatically, it will default to 'ALL-TENANTS'.
        */
        choice(
            name: 'DEPLOY_TARGET', 
            choices: ['tenant-f', 'tenant-a', 'tenant-b', 'tenant-c', 'tenant-d', 'tenant-e'], 
            description: 'Select "ALL-TENANTS" for full automation or a specific tenant for manual override.'
        )
    }

    environment {
        // AWS Secret IDs from your Jenkins Credentials
        AWS_ACCESS_KEY_ID     = credentials('access-keys')
        AWS_SECRET_ACCESS_KEY = credentials('secret-access')
        
        // Configuration
        AWS_DEFAULT_REGION    = 'us-east-1' 
        S3_BUCKET             = 'testwebsitefiles'
        
        // GitHub Details
        REPO_URL              = 'https://github.com/Testawsdevops007/websitefilestest.git'
        GITHUB_CRED_ID        = 'github-creds' 
    }

    stages {
        stage('Checkout Source') {
            steps {
                echo "Pulling latest code from GitHub..."
                git branch: 'main', 
                    credentialsId: "${GITHUB_CRED_ID}", 
                    url: "${REPO_URL}"
            }
        }

        stage('Deploy to S3') {
            steps {
                script {
                    // Define the list of all your tenants here
                    def allTenants = ['tenant-a', 'tenant-b', 'tenant-c', 'tenant-d', 'tenant-e','tenant-f']
                    def targets = []

                    // Logic: If 'ALL-TENANTS' is selected, loop through everyone.
                    // Otherwise, only deploy to the selected one.
                    if (params.DEPLOY_TARGET == 'ALL-TENANTS') {
                        echo "🚨 Automated/Full Deployment detected. Updating ALL tenants..."
                        targets = allTenants
                    } else {
                        echo "🎯 Manual Deployment detected. Updating only: ${params.DEPLOY_TARGET}"
                        targets = [params.DEPLOY_TARGET]
                    }

                    // The actual Upload Loop
                    for (tenant in targets) {
                        def s3Path = "s3://${S3_BUCKET}/${tenant}/websitefiles/"
                        echo "Uploading to ${s3Path}..."
                        
                        sh "aws s3 sync . ${s3Path} --exclude '.git/*' --exclude 'Jenkinsfile' --delete"
                    }
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful for: ${params.DEPLOY_TARGET}"
        }
        failure {
            echo "❌ Deployment Failed. Check AWS IAM permissions or S3 Bucket Policy."
        }
    }
}
