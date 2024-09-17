pipeline {
    agent any
   
    environment {
        //TERRAFORM_WORKSPACE = "/var/lib/jenkins/workspace/tool_deploy/prometheus_infra/"
        INSTALL_WORKSPACE = "/var/lib/jenkins/workspace/tomcat-deploy/tomcat/"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Priyanshu498/Final-tomcat.git'
            }
        }
        
        stage('Tool Deploy') {
            when {
                expression { params.ACTION == 'apply' }
            }
            steps {
                // Deploy Prometheus
                sh '''cd /var/lib/jenkins/workspace/tomcat-deploy/tomcat/
                ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook playbook.yml    '''
            }
        }
    }
    post {
        success {
            // Actions to take if the pipeline is successful
            echo 'Succeeded!'
        }
        failure {
            // Actions to take if the pipeline fails
            echo 'Failed!'
        }
    }
}



