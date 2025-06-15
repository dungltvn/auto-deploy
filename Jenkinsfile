pipeline {
    agent any

    environment {
        // Đảm bảo Ansible được cài đặt trên Jenkins agent.
        // Hoặc bạn có thể sử dụng Docker agent với image có sẵn Ansible.
        // Thay đổi đường dẫn nếu Ansible không nằm ở đây.
        ANSIBLE_BIN = '/usr/bin/ansible-playbook'
    }

    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/dungltvn/auto-deploy.git'
                // Hoặc đơn giản hơn: checkout scm (nếu bạn đã cấu hình SCM ở trên)
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    // Sử dụng SSH Agent Plugin để đưa SSH key vào môi trường
                    withCredentials([sshUserPrivateKey(credentialsId: 'jenkins_ansible_ssh_key', keyFileVariable: 'ANSIBLE_SSH_KEY')]) {
                        sh """
                        chmod 600 \$ANSIBLE_SSH_KEY
                        ${ANSIBLE_BIN} -i hosts test.yml --private-key \$ANSIBLE_SSH_KEY
                        """
                    }
                }
            }
        }
    }
}
