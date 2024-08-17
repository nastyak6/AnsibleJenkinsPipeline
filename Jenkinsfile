pipeline {
    agent any
    parameters {
        choice(name: 'host', choices: ['worker1', 'worker2'], description: 'Choose the host to configure')
        booleanParam(name: 'install_curl', defaultValue: true, description: 'Install curl')
        booleanParam(name: 'install_htop', defaultValue: true, description: 'Install htop')
    }
    stages {
        stage('Prepare Ansible') {
            steps {
                script {
                    // Create Ansible inventory file based on the selected host
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

        stage('Install Packages') {
            when {
                anyOf {
                    expression { return params.install_curl }
                    expression { return params.install_htop }
                }
            }
            steps {
                script {
                    def packages = []
                    if (params.install_curl) {
                        packages.add('curl')
                    }
                    if (params.install_htop) {
                        packages.add('htop')
                    }
                    ansiblePlaybook(
                        playbook: 'tasks/playbook.yml',
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
