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

        stage('Verify Files') {
            steps {
                // List files in the workspace to verify paths
                sh 'ls -R /var/lib/jenkins/workspace/tom'
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                // Create and activate the virtual environment
                sh '''
                python3 -m venv /var/lib/jenkins/workspace/tom/myenv
                bash -c "source /var/lib/jenkins/workspace/tom/myenv/bin/activate && pip install --upgrade pip"
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependencies from requirements.txt
                sh '''
                bash -c "source /var/lib/jenkins/workspace/tom/myenv/bin/activate && pip install -r /var/lib/jenkins/workspace/tom/requirements.txt"
                '''
            }
        }

        stage('Dry Run Playbook') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws-credentials-id']]) {
                    // Perform a dry run (check mode) of the Ansible playbook
                    sh '''
                    bash -c "source /var/lib/jenkins/workspace/tom/myenv/bin/activate && ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml --check -e ansible_python_interpreter=/var/lib/jenkins/workspace/tom/myenv/bin/python"
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
                    bash -c "source /var/lib/jenkins/workspace/tom/myenv/bin/activate && ansible-playbook -i /opt/aws_ec2.yml tomcat/tests/test.yml"
                    '''
                }
            }
        }
    }

    post {
        always {
            // Cleanup steps: Deactivate virtual environment and any additional steps if required
            sh '''
            bash -c "source /var/lib/jenkins/workspace/tom/myenv/bin/activate && deactivate || true"
            '''
        }
    }
}
