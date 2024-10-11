pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
        AWS_DEFAULT_REGION    = 'ap-south-1'
    }

    stages {
        stage('Terraform Init') {
            steps {
                script {
                    {
                        dir(TERRAFORM_DIR) {
                            sh '''
                            export AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID}
                            export AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY}
                            cd TERRAFORMENV
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
                     {
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
}
    // post {
    //     always {
    //         cleanWs()  // Clean up workspace after the pipeline is done
    //     }
    //     failure {
    //         echo "Terraform deployment failed!"
    //     }
    //     success {
    //         echo "Terraform deployment succeeded!"
    //     }
    // }

