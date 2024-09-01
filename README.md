# Ansible Jenkins Pipeline

## Overview

This repository provides a Jenkins pipeline integrated with Ansible for automating the setup and configuration of remote hosts. The pipeline is designed to streamline the deployment process by automating the installation of required packages, configuration of system files, and execution of various tasks on specified remote hosts.

## Features

- **Automated Setup**: The pipeline uses Ansible playbooks to automate the setup and configuration of remote hosts.
- **Multi-Branch Pipeline**: Each branch in the repository can be configured with its own set of parameters and tasks.
- **Parameterization**: Users can provide specific parameters such as IP addresses, usernames, and package lists to customize the setup process.
- **Package Installation**: The pipeline includes stages to install essential packages and configure them on remote hosts.
- **Configuration Management**: Dedicated configuration files are applied to the remote hosts based on the parameters provided.
- **Error Handling**: The pipeline includes checks and validations to ensure that the setup process completes successfully.

## Repository Structure

- **`Jenkinsfile`**: The main Jenkins pipeline script that defines the stages and steps for the automation process.
- **`ansible/`**: Contains the Ansible playbooks and roles used to configure the remote hosts.
- **`hosts.ini`**: Inventory file that specifies the remote hosts and their associated parameters.
- **`README.md`**: This file provides an overview of the project and instructions for getting started.
- **`CONTRIBUTORS.md`**: List of contributors to the project, including their full names and nicknames.

## Prerequisites

Before running the pipeline, ensure you have the following:

- Jenkins installed and configured.
- Ansible installed on the Jenkins master or agent.
- Remote hosts accessible via SSH.
- Proper credentials and SSH keys set up for the remote hosts.

## Getting Started

1. **Clone the Repository**: 
   ```bash
   git clone https://github.com/nastyak6/AnsibleJenkinsPipeline.git
   cd AnsibleJenkinsPipeline
2. **Set Up Jenkins Pipeline**:

    Add the repository to your Jenkins server as a new pipeline.
    Configure the Jenkinsfile with the required parameters.

3. **Configure Remote Hosts**:

    Update the hosts.ini file with the IP addresses, usernames, and passwords/SSH keys for your remote hosts.

4. **Run the Pipeline**:

    Trigger the Jenkins pipeline to start the automated setup process on the remote hosts.
