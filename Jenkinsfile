pipeline {
    agent any
    
    parameters {
        string(name: 'HOST_IP', defaultValue: '', description: 'Remote host IP address')
        choice(name: 'HOST_TYPE', choices: ['server', 'desktop', 'laptop'], description: 'Type of remote host')
        string(name: 'USERNAME', defaultValue: '', description: 'Username to create')
        password(name: 'PASSWORD', defaultValue: '', description: 'Password for the user')
        string(name: 'SSH_KEY', defaultValue: '', description: 'Golden SSH key for future remote connections')
        string(name: 'PACKAGES', defaultValue: '', description: 'Comma-separated list of packages to install')
        text(name: 'CONFIG_FILES', defaultValue: '', description: 'Dedicated config files as JSON or other formats')
    }
    
    stages {
        stage('Prepare Environment') {
            steps {
                script {
                    env.PACKAGES_LIST = params.PACKAGES.split(',')
                }
            }
        }
        
        stage('Ansible Setup') {
            steps {
                ansiblePlaybook(
                    playbook: 'playbooks/setup-host.yml',
                    inventory: 'localhost,',
                    extras: '-e "host_ip=${params.HOST_IP} username=${params.USERNAME} password=${params.PASSWORD} ssh_key=${params.SSH_KEY} packages=${env.PACKAGES_LIST} config_files=${params.CONFIG_FILES}"'
                )
            }
        }
    }
}
