pipeline {
    agent any

    environment {
        // Set your AWS credentials and region
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        AWS_REGION = 'us-west-2' // Change this to your desired region

        // Define the Terraform version to use
        TERRAFORM_VERSION = '1.0.0'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Terraform Init') {
            steps {
                script {
                    // Initialize Terraform
                    sh """
                        terraform init
                    """
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    // Generate Terraform execution plan
                    sh """
                        terraform plan -out=tfplan
                    """
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    // Apply the Terraform configuration
                    sh """
                        terraform apply -auto-approve tfplan
                    """
                }
            }
        }
    }

    post {
        always {
            script {
                // Cleanup and remove the plan file
                sh """
                    rm -f tfplan
                """
            }
        }

        success {
            echo 'Terraform apply completed successfully!'
        }

        failure {
            echo 'Terraform apply failed!'
        }
    }
}
