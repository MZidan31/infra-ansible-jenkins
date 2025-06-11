pipeline {
  agent any
  tools { ansible 'ansible' }
  environment { ANSIBLE_CONFIG = "${WORKSPACE}/ansible.cfg" }
  stages {
    stage('Checkout') {
      steps { git url: 'https://github.com/MZidan31/infra-ansible-jenkins.git', branch: 'main' }
    }
    stage('SSH Test') {
      steps {
        sshagent(['ansible-ssh-key']) {
          sh "ssh -o StrictHostKeyChecking=no serda@192.168.239.128 exit"
        }
      }
    }
    stage('Deploy NGINX') {
      steps {
        sshagent(['ansible-ssh-key']) {
          ansiblePlaybook(
            credentialsId: 'ansible-ssh-key',
            disableHostKeyChecking: true,
            installation: 'ansible',
            inventory: 'hosts.ini',
            playbook: 'playbook.yml',
            colorized: true
          )
        }
      }
    }
  }
  post { always { cleanWs() } }
}
