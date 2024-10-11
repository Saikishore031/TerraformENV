pipeline {
    agent any

    environment {
        // Terraform working directory (adjust this to match your setup)
        TERRAFORM_DIR = 'TerraformENV'
        // Jenkins credentials ID where AWS credentials are stored
        AWS_CREDENTIALS_ID = 'awscred'
        // Jenkins credentials ID where GitHub credentials (Personal Access Token or SSH key) are stored
        GITHUB_CREDENTIALS_ID = 'envgithub'
        // GitHub repository URL
        GITHUB_REPO_URL = 'https://github.com/Saikishore031/TerraformENV.git'
        // GitHub branch (optional, default to 'main')
        GITHUB_BRANCH = 'main'
    }

    stages {
        stage('Terraform Init') {
            steps {
                script {
                    // With AWS credentials from Jenkins
                    withCredentials([usernamePassword(credentialsId: AWS_CREDENTIALS_ID, 
                                                      usernameVariable: 'AWS_ACCESS_KEY_ID', 
                                                      passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        dir(TERRAFORM_DIR) {
                            sh '''
                            export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
                            export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
                            terraform init
                            '''
                        }
                    }
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: AWS_CREDENTIALS_ID, 
                                                      usernameVariable: 'AWS_ACCESS_KEY_ID', 
                                                      passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        dir(TERRAFORM_DIR) {
                            sh '''
                            export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
                            export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
                            terraform plan
                            '''
                        }
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: AWS_CREDENTIALS_ID, 
                                                      usernameVariable: 'AWS_ACCESS_KEY_ID', 
                                                      passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        dir(TERRAFORM_DIR) {
                            sh '''
                            export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
                            export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
                            terraform apply -auto-approve
                            '''
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()  // Clean up workspace after the pipeline is done
        }
        failure {
            echo "Terraform deployment failed!"
        }
        success {
            echo "Terraform deployment succeeded!"
        }
    }
}
