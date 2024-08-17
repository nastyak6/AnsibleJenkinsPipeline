pipeline {
    agent any
    parameters {
        string(name: 'host_ip', description: 'Remote Host IP Address')
        string(name: 'username', description: 'Username for the remote host')
        password(name: 'password', description: 'Password for the user')
        text(name: 'ssh_key', description: 'SSH Key for remote connection')
        text(name: 'packages', description: 'List of packages to install')
        text(name: 'config_files', description: 'Dedicated config files for the software')
    }
    stages {
        stage('Prepare Ansible') {
            steps {
                script {
                    // Save SSH key and create inventory file
                    writeFile file: 'ssh_key', text: params.ssh_key
                    sh 'chmod 600 ssh_key'
                    
                    // Create Ansible inventory file
                    writeFile file: 'inventory', text: "[remote]\n${params.host_ip} ansible_user=${params.username} ansible_ssh_private_key_file=./ssh_key"
                }
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                ansiblePlaybook(
                    playbook: 'playbook.yml',
                    inventory: 'inventory',
                    extraVars: [
                        host_ip: "${params.host_ip}",
                        username: "${params.username}",
                        password: "${params.password}",
                        packages: "${params.packages}",
                        config_files: "${params.config_files}"
                    ]
                )
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
