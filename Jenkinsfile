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
                    sh '''
                    pwd
                    ls -ltr
                    terraform init
                    '''
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                script {
                    dir('TERRAFORMENV') {
                        sh '''
                        terraform plan
                        '''
                    }
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    dir('TERRAFORMENV') {
                        sh '''
                        terraform apply -auto-approve
                        '''
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
}
