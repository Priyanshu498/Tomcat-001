pipeline {
    agent any

    environment {
        AWS_CREDENTIALS = credentials('aws-credentials-id') 
    }

    stages {
        stage('Git Checkout') {
            steps {      
                git branch: 'main', url: 'https://github.com/Priyanshu498/Final-tomcat.git'
            }
        }

        stage('Dry Run Playbook') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials-id']]) {         
                    sh '''
                    ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml  -e ansible_python_interpreter=/usr/bin/python3 --check
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
                    ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml  -e ansible_python_interpreter=/usr/bin/python3
                    '''
                }
            }
        }
    }

    post {
        success {
            // Notify success
            echo 'Playbook executed successfully.'
        }
        failure {
            // Notify failure
            echo 'Playbook execution failed.'
        }
    }
}

