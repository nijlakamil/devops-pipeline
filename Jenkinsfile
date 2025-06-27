pipeline {
  agent any

  stages {
    stage('Terraform Init') {
      steps {
        dir('terraform') {
          sh 'terraform init'
        }
      }
    }
    stage('Terraform Apply') {
      steps {
        dir('terraform') {
          sh 'terraform apply -auto-approve'
        }
      }
    }
    stage('Ansible Run') {
      steps {
        sshagent (credentials: ['ansible-key']) {
          sh 'ansible-playbook -i inventory.ini ansible/install_web.yml'
        }
      }
    }
    stage('Verify Website') {
      steps {
        sh 'curl http://74.235.176.10'
      }
    }
  }
}
