pipeline {
    agent any

    environment {
        // Jenkins credentials ID for AWS access
        AWS_CREDENTIALS = credentials('aws-credentials-id')
        SSH_KEY = credentials('ansible-ssh-key')
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Priyanshu498/Final-tomcat.git'
            }
        }

        stage('List Files') {
            steps {
                sh 'ls -R'
            }
        }

        stage('Dryrun Playbook') {
            steps {
                withCredentials([file(credentialsId: 'aws-credentials-id', variable: 'AWS_CREDENTIALS_FILE'), 
                                 sshUserPrivateKey(credentialsId: 'ansible-ssh-key', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                    sh '''
                    ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml --check --private-key=$SSH_PRIVATE_KEY
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
                withCredentials([file(credentialsId: 'aws-credentials-id', variable: 'AWS_CREDENTIALS_FILE'), 
                                 sshUserPrivateKey(credentialsId: 'ansible-ssh-key', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                    sh '''
                    ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml --private-key=$SSH_PRIVATE_KEY
                    '''
                }
            }
        }
    }
}
