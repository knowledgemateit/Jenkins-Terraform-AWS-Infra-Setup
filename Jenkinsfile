pipeline {
    agent any

    parameters {
        choice(name: 'action', choices: ['apply', 'destroy'], description: 'Pick the Terraform action to perform')
    }

    environment {
        // Ensuring Terraform doesn't ask for input during automation
        TF_IN_AUTOMATION = 'true'
    }

    stages {
        stage('Checkout') {
            steps {
                // Simplified checkout syntax
                git branch: 'main', url: 'https://github.com/knowledgemateit/Terraform-Jenkins.git'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Plan') {
            steps {
                // Save the plan to a file to ensure consistency in the next stage
                sh "terraform plan -out=tfplan"
            }
        }

        stage('Approval Gate') {
            // Only require approval for apply; destroy can be handled similarly if preferred
            when { expression { params.action == 'apply' } }
            steps {
                input message: "Review the plan above. Proceed with ${params.action}?", ok: "Yes, proceed"
            }
        }

        stage('Terraform Action') {
            steps {
                script {
                    echo "Executing Terraform ${params.action}..."
                    if (params.action == 'apply') {
                        // Apply the specific plan file generated earlier
                        sh 'terraform apply tfplan'
                    } else {
                        sh 'terraform destroy --auto-approve'
                    }
                }
            }
        }
    }

    post {
        always {
            // Cleanup the plan file to avoid sensitive data lingering
            sh 'rm -f tfplan'
        }
    }
}
