pipeline {
    agent { label 'ansible' } // Your Ansible server configured as Jenkins agent 
    
    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', credentialsId: 'GitHubCred', url: 'https://github.com/Supriya-lr/student-operation-repo.git'
            }
        }

        stage('Ping Tomcat Server'){
            steps{
                withCredentials([file(credentialsId: 'AWS_EC2_PEM', variable: 'aws_pem_file')]) {
                sh """ 
                     ansible -i ansible-scripts/inventory.ini tomcatServer -m ping -e ansible_ssh_private_key_file=${aws_pem_file} --ssh-common-args='-o StrictHostKeyChecking=no'
                """
                }
            }
        }
        
        stage('Install Tomcat'){
            steps{
                withCredentials([file(credentialsId: 'AWS_EC2_PEM', variable: 'aws_pem_file')]){
                    sh "ansible-playbook -i ansible-scripts/inventory.ini tomcatServer ansible-scripts/playbook.yaml -e ansible_ssh_private_key_file=${aws_pem_file} --ssh-common-args='-o StrictHostKeyChecking=no'"
                }
            }
        }
    }    
}