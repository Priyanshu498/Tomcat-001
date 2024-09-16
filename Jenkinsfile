pipeline {
    agent any

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
                // Use SSH credentials to run the dry run of the Ansible playbook
                withCredentials([sshUserPrivateKey(credentialsId: 'ubuntu', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                    sh '''
                    ansible-playbook -i Final-tomcat/tomcat/tests/inventory Final-tomcat/tomcat/tests/test.yml --check
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
                // Use SSH credentials to run the Ansible playbook
                withCredentials([sshUserPrivateKey(credentialsId: 'ubuntu', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                    sh '''
                    ansible-playbook -i Final-tomcat/tomcat/tests/inventory Final-tomcat/tomcat/tests/test.yml
                    '''
                }
            }
        }
    }
}
