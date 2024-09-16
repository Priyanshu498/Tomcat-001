pipeline {
    agent any

    environment {
        AWS_CREDENTIALS = credentials('aws-credentials-id') // Make sure the ID matches
    }

    stages {
        stage('Git Checkout') {
            steps {
                // Checkout the code from your Git repository
                git branch: 'main', url: 'https://github.com/Priyanshu498/Final-tomcat.git'
            }
        }

        stage('List Files') {
            steps {
                // List files in the repository to verify the checkout
                sh 'ls -R'
            }
        }

        stage('Dryrun Playbook') {
            steps {
                // Use AWS credentials to run the dry run of the Ansible playbook
                withCredentials([awsCredentials(credentialsId: 'aws-credentials-id')]) {
                    sh '''
                    ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml --check
                    '''
                }
            }
        }

        stage('Execute Playbook') {
            input {
                message "Do you want to perform apply?"
                ok "Yes"
            }
            steps {
                // Use AWS credentials to run the Ansible playbook
                withCredentials([awsCredentials(credentialsId: 'aws-credentials-id')]) {
                    sh '''
                    ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml
                    '''
                }
            }
        }
    }
}

