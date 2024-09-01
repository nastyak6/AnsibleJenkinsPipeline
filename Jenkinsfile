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
        stage('Install sshpass') {
            steps {
                script {
                    if (sh(script: "command -v sshpass", returnStatus: true) != 0) {
                        sh "sudo apt-get update && sudo apt-get install sshpass -y"
                    }
                }
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
