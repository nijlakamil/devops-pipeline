pipeline {
  agent any

  environment {
    ARM_CLIENT_ID       = credentials('ARM_CLIENT_ID')
    ARM_CLIENT_SECRET   = credentials('ARM_CLIENT_SECRET')
    ARM_SUBSCRIPTION_ID = credentials('ARM_SUBSCRIPTION_ID')
    ARM_TENANT_ID       = credentials('ARM_TENANT_ID')
  }

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
          sh '''
            # Add public IP to known_hosts to avoid verification failure
            ssh-keyscan -H 74.235.176.10 >> ~/.ssh/known_hosts

            # Run Ansible playbook
            ansible-playbook -i inventory.ini ansible/install_web.yml
          '''
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
