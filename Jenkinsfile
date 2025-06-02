pipeline {
    agent any

    environment {
        ANSIBLE_FORCE_COLOR = 'true'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy NGINX via Ansible') {
            steps {
                sshagent(credentials: ['test2']) {
                    ansiColor('xterm') {
                        sh 'ansible-playbook nginx.yml -i inventory.ini'
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
