pipeline {
    agent any
    parameters {
        choice(name: 'ACTION', choices: ['roles'], description: 'Select action: roles')
    }
    environment {
        SSH_KEY = credentials('ssh-credentials-id') // Updated Jenkins credentials for SSH key
    }
    stages {
        stage('Clone Repository') {
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
            when {
                expression { params.ACTION == 'roles' }
            }
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh-credentials-id', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                    sh '''
                    ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml --check
                    '''
                }
            }
        }
        stage('Execute Playbook') {
            when {
                expression { params.ACTION == 'roles' }
            }
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh-credentials-id', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                    sh '''
                     ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml
                    '''
                }
            }
        }
    }
    post {
        always {
            node('any') {
                cleanWs()
            }
        }
    }
}



