pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git(
                    branch: 'main',
                    credentialsId: 'ansible-ssh-key',
                    url: 'https://github.com/MZidan31/infra-ansible-jenkins.git'
                )
            }
        }

        stage('Deploy NGINX via Ansible') {
            steps {
                sshagent(credentials: ['test2']) {
                    ansiColor('xterm') {
                        sh '''
                            ansible-playbook nginx.yml \
                            -i inventory \
                            --key-file ~/.ssh/id_rsa
                        '''
                    }
                }
            }
        }
    }

    post {
        failure {
            echo '❌ Deployment gagal. Silakan cek error log di konsol output.'
        }
        success {
            echo '✅ Deployment berhasil!'
        }
    }
}
