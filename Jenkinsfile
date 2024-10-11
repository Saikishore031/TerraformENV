pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        AWS_DEFAULT_REGION    = 'ap-south-1'
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
                            pwd
                            ls -ltr
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
