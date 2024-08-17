pipeline {
    agent any
    parameters {
        choice(name: 'host_choice', choices: ['node1', 'node2', 'node3', 'node4'], description: 'Choose the remote host')
        booleanParam(name: 'install_packages', defaultValue: true, description: 'Install curl and htop')
    }
    stages {
        stage('Prepare Ansible') {
            steps {
                script {
                    // Extract the host information from hosts.ini
                    def hostInfo = readFile('hosts.ini').split('\n').find { it.contains(params.host_choice) }
                    def username = hostInfo.split(' ')[1].split('=')[1]
                    def password = hostInfo.split(' ')[2].split('=')[1]

                    // Create Ansible inventory file
                    writeFile file: 'inventory', text: "[remote]\n${params.host_choice} ansible_user=${username} ansible_password=${password}"
                }
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                script {
                    if (params.install_packages) {
                        def packages = "curl,htop"
                        ansiblePlaybook(
                            playbook: 'playbook.yml',
                            inventory: 'inventory',
                            extraVars: [
                                host_ip: "${params.host_choice}",
                                username: "${username}",
                                password: "${password}",
                                packages: "${packages}"
                            ]
                        )
                    } else {
                        ansiblePlaybook(
                            playbook: 'playbook.yml',
                            inventory: 'inventory',
                            extraVars: [
                                host_ip: "${params.host_choice}",
                                username: "${username}",
                                password: "${password}"
                            ]
                        )
                    }
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
