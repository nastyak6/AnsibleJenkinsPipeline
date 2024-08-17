pipeline {
    agent any
    parameters {
        choice(name: 'selected_host', choices: ['node1', 'node2', 'node3', 'node4', 'node_app'], description: 'Select the Host')
    }
    stages {
        stage('Prepare Ansible') {
            steps {
                script {
                    // Use the selected host's credentials from hosts.ini
                    def hostInfo = readFile('hosts.ini').readLines().findAll { it.startsWith("${params.selected_host} ") }
                    def ansible_user = hostInfo.find { it.contains('ansible_user=') }?.split('=')[1]?.trim()
                    def ansible_password = hostInfo.find { it.contains('ansible_password=') }?.split('=')[1]?.trim()

                    // Create Ansible inventory file for the selected host
                    writeFile file: 'inventory', text: """
                    [selected_host]
                    ${params.selected_host} ansible_user=${ansible_user} ansible_password=${ansible_password} ansible_ssh_private_key_file=./id_rsa
                    """
                }
            }
        }
        stage('Install Packages and Configurations') {
            steps {
                script {
                    // Define the general packages and configuration files
                    def packages = "package1,package2"
                    def config_files = "/path/to/config1,/path/to/config2"

                    // Run Ansible Playbook to install packages and configure files
                    ansiblePlaybook(
                        playbook: 'playbook.yml',
                        inventory: 'inventory',
                        extraVars: [
                            selected_host: "${params.selected_host}",
                            packages: packages,
                            config_files: config_files
                        ]
                    )
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
