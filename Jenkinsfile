pipeline {
    agent any

    environment {
        // Define environment variables if needed
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout the source code from Git
                git 'https://github.com/Priyanshu498/Final-tomcat.git'
            }
        }

        stage('List Files') {
            steps {
                // List files in the workspace
                sh 'ls -R'
            }
        }

        stage('Dryrun Playbook') {
            steps {
                // Use SSH credentials for Ansible playbook execution
                withCredentials([sshUserPrivateKey(credentialsId: 'ansible-ssh-key', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        ansible-playbook -i tomcat/tests/inventory tomcat/tests/test.yml --check
                    '''
                }
            }
        }

        stage('Execute Playbook') {
            steps {
                // Execute the Ansible playbook
                withCredentials([sshUserPrivateKey(credentialsId: 'ansible-ssh-key', keyFileVariable: 'SSH_KEY')]) {
                    sh '''
                        ansible-playbook -i tomcat/tests/inventory tomcat/tests/test.yml
                    '''
                }
            }
        }
    }

    post {
        always {
            // Actions to perform after the pipeline finishes
            cleanWs()
        }
    }
}

