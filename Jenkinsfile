pipeline {
    agent any

    environment {
        AWS_CREDENTIALS = credentials('aws-credentials-id') // Ensure this ID matches
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

        stage('Setup Virtual Environment') {
            steps {
                sh '''
                python3 -m venv myenv
                . myenv/bin/activate
                pip install boto3 botocore
                '''
            }
        }

        stage('Dryrun Playbook') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials-id']]) {
                    sh '''
                    . myenv/bin/activate
                    ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml --check -e ansible_python_interpreter=/var/lib/jenkins/workspace/tom/myenv/bin/python
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
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials-id']]) {
                    sh '''
                    . myenv/bin/activate
                    ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml -e ansible_python_interpreter=/var/lib/jenkins/workspace/tom/myenv/bin/python
                    '''
                }
            }
        }
    }
}

