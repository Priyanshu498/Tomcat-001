pipeline {
    agent any
    parameters {
        choice(name: 'ACTION', choices: ['apply', 'destroy'], description: 'Select action: apply or destroy')
    }
    environment {
        TERRAFORM_WORKSPACE = "/var/lib/jenkins/workspace/tool_deploy/tomcat-infra/"
        INSTALL_WORKSPACE = "/var/lib/jenkins/workspace/tool_deploy/tomcat/"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Priyanshu498/Final-tomcat.git'
            }
        }
        stage('Terraform Init') {
            steps {
                // Initialize Terraform
                sh """
                    cd ${TERRAFORM_WORKSPACE}
                    terraform init
                """
            }
        }
        stage('Terraform Plan') {
            steps {
                // Run Terraform plan
                sh """
                    cd ${TERRAFORM_WORKSPACE}
                    terraform plan
                """
            }
        }
        stage('Approval For Apply') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                // Prompt for approval before applying changes
                input message: 'Do you want to apply Terraform changes?'
            }
        }
        stage('Terraform Apply') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                // Run Terraform apply
                sh """
                    cd ${TERRAFORM_WORKSPACE}
                    terraform apply -auto-approve
                """
            }
        }
        stage('Approval for Destroy') {
            when {
                expression { params.ACTION == 'destroy' }
            }
            steps {
                // Prompt for approval before destroying resources
                input message: 'Do you want to Terraform Destroy?'
            }
        }
        stage('Terraform Destroy') {
            when {
                expression { params.ACTION == 'destroy' }
            }
            steps {
                // Run Terraform apply
                sh """
                cd ${env.TERRAFORM_WORKSPACE}
                terraform apply -auto-approve
                mkdir -p ${env.INSTALL_WORKSPACE}  # Create the directory if it doesn't exist
                sudo cp ${env.TERRAFORM_WORKSPACE}/tom-1-key.pem ${env.INSTALL_WORKSPACE}/
                sudo chown jenkins:jenkins ${env.INSTALL_WORKSPACE}/tom-1-key.pem
                sudo chmod 400 ${env.INSTALL_WORKSPACE}/tom-1-key.pem
                """
            }
        }
        stage('Tool Deploy') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                sshagent(credentials: ['tomcat']) {
                    script {
                        sh """
                            ansible-playbook -i ./tomcat_roles/Tomcat/aws_ec2.yml ./tomcat_roles/Tomcat/playbook.yml
                        """
                    }
                }
            }
        }
    }
    post {
        success {
            // Actions to take if the pipeline is successful
            echo 'Pipeline Succeeded!'
        }
        failure {
            // Actions to take if the pipeline fails
            echo 'Pipeline Failed!'
        }
    }
}
