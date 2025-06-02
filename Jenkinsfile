pipeline {
    agent any

    environment {
        ANSIBLE_FORCE_COLOR = 'true'
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/MZidan31/infra-ansible-jenkins.git'
            }
        }

        stage('Deploy NGINX via Ansible') {
            steps {
                sshagent(credentials: ['ansible-ssh-key']) {
                    ansiColor('xterm') {
                        sh '''
                            cd $WORKSPACE
                            ansible-playbook -i ansible/inventory.ini ansible/nginx.yml --ssh-extra-args="-o StrictHostKeyChecking=no"
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment NGINX berhasil!'
        }
        failure {
            echo '❌ Deployment gagal. Silakan cek error log di konsol output.'
        }
    }
}
