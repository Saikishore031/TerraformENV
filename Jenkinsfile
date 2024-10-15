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
                    sh '''
                    terraform plan -var-file=test.tfvars -no-color
                    '''
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script {
                    // Apply the changes and capture the public IP from the output
                    def output = sh(script: "terraform apply -auto-approve -var-file=test.tfvars -no-color && terraform output -json", returnStdout: true).trim()
                    
                    // Parse the JSON output and extract the public IP
                    def terraformOutput = readJSON text: output
                    def publicIp = terraformOutput.instance_public_ip.value
                    
                    // Save the public IP as an environment variable to pass to the next pipeline
                    env.PUBLIC_IP = publicIp
                    echo "Public IP: ${env.PUBLIC_IP}"
                }
            }
        }

        //stage('Trigger Ansible Pipeline') {
        //    steps {
        //        script {
                    // Trigger the Ansible pipeline and pass the public IP
        //            build job: 'ansible-pipeline-job', parameters: [string(name: 'PUBLIC_IP', value: "${env.PUBLIC_IP}")]
        //        }
        //    }
        //}
    }
}
