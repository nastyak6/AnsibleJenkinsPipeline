pipeline {
    agent any
    parameters {
        choice(name: 'host', choices: ['worker1', 'worker2'], description: 'Choose the host to configure')
        booleanParam(name: 'install_wget', defaultValue: true, description: 'Install wget')
        booleanParam(name: 'install_top', defaultValue: true, description: 'Install top')
    }
    stages {
        stage('Prepare Ansible') {
            steps {
                script {
                    writeFile file: 'inventory', text: "[remote]\n${params.host} ansible_user=jenkins ansible_password=jenkins"
                }
            }
        }
        stage('Verify Inventory') {
            steps {
                script {
                    sh 'cat inventory'
                }
            }
        }
        stage('Prepare Ansible Config') {
            steps {
                writeFile file: 'ansible.cfg', text: '''
                [defaults]
                inventory = inventory
                host_key_checking = False
                timeout = 60

                [privilege_escalation]
                become = true
                become_method = sudo
                become_user = root
                become_ask_pass = false

                [ssh_connection]
                ssh_args = -o ControlMaster=auto -o ControlPersist=60s
                scp_if_ssh = True
                pipelining = True
                '''
            }
        }

        stage('Install Packages') {
            when {
                anyOf {
                    expression { return params.install_wget }
                    expression { return params.install_top }
                }
            }
            steps {
                script {
                    def packages = []
                    if (params.install_wget) {
                        packages.add('wget')
                    }
                    if (params.install_top) {
                        packages.add('top')
                    }
                    ansiblePlaybook(
                        playbook: 'tasks/install_packages.yml',
                        inventory: 'inventory',
                        extraVars: [
                            packages: "${packages.join(',')}"
                        ]
                    )
                }
            }
        }
        stage('Apply Configuration') {
            steps {
                ansiblePlaybook(
                    playbook: 'apply_config.yml',
                    inventory: 'inventory'
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
