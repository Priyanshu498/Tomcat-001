pipeline {
    agent any

    environment {
        AWS_CREDENTIALS = credentials('aws-credentials-id') // Ensure this ID matches your Jenkins credentials ID
    }

    stages {
        stage('Git Checkout') {
            steps {
                // Checkout the latest code from the 'main' branch
                git branch: 'main', url: 'https://github.com/Priyanshu498/Final-tomcat.git'
            }
        }

        
        stage('Setup Virtual Environment') {
            steps {
                // Create a Python virtual environment and install dependencies
                sh '''
                python3 -m venv myenv
                . myenv/bin/activate
                pip install boto3 botocore  # Install required AWS libraries
                '''
            }
        }

        stage('Dry Run Playbook') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials-id']]) {
                    // Perform a dry run (check mode) of the Ansible playbook
                    sh '''
                    . myenv/bin/activate
                    ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml -e ansible_python_interpreter=/var/lib/jenkins/workspace/tom/myenv/bin/python

                    '''
                }
            }
        }

        stage('Execute Playbook') {
            input {
                // Prompt for manual approval before proceeding with the actual execution
                message "Do you want to perform apply?"
                ok "Yes"
            }
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials-id']]) {
                    // Execute the Ansible playbook without check mode
                    sh '''
                    . myenv/bin/activate
                   ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml -e ansible_python_interpreter=/var/lib/jenkins/workspace/tom/myenv/bin/python

                    '''
                }
            }
        }
    }

    post {
        always {
            // Cleanup steps: Deactivate virtual environment and any additional steps if required
            sh '''
            deactivate || true  # Ensure that the deactivate command doesn't fail the pipeline
            '''
        }
    }
}

