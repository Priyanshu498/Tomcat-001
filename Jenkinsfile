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

        stage('Dry Run Playbook') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials-id']]) {
                    // Perform a dry run (check mode) of the Ansible playbook
                    sh '''
                    ansible-playbook -i tomcat/tests/inventory tomcat/tests/test.yml 
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
                    ansible-playbook -i  tomcat/tests/inventory tomcat/tests/test.yml
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
