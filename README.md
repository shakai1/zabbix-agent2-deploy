# Zabbix Agent Configuration and Registration

This repository contains an Ansible playbook that:

- Installs and configures Zabbix Agent 2 on target hosts.
- Configures secure PSK authentication for encrypted communication.
- Automatically registers hosts and links templates in the Zabbix dashboard.

## Features

- Installs Zabbix Agent 2 and required plugins.
- Configures the agent to communicate securely with the Zabbix server.
- Creates an auto-registration action for Linux hosts in Zabbix.
- Adds the Linux server group and links templates to hosts.

## Prerequisites

1. Ansible installed on the control node.
2. Zabbix server with API access enabled.
3. Python `py-zabbix` library installed.

## Usage

1. **Clone the Repository:**

   git clone <repository_url>
   cd <repository_name>

2. **Update Variables:**

   Edit the `vars` section in the playbook to match your environment. Replace the placeholder values like Zabbix server IP, proxy settings, and repository details.

3. **Edit Host Group Task:**

   The task:

   - name: Add Linux servers group to hosts
     zabbix_hostgroup:
       server_url: "http://{{ zabbix_server_ip }}/zabbix/api_jsonrpc.php"
       login_user: "{{ zabbix_admin_username }}"
       login_password: "{{ zabbix_admin_password }}"
       name: "Linux servers"
       state: present

   Modify the `name` field in the `zabbix_hostgroup` task according to your needs. This should match the name of the host group you want to assign hosts to in your Zabbix configuration.

4. **Ensure Secret File:**

   The playbook includes a task to decrypt the `secret.psk` file required for secure communication. Make sure to create and encrypt the `secret.psk` file using Ansible Vault:

   ansible-vault encrypt secret.psk

5. **Run the Playbook:**

   Run the playbook with:

   ansible-playbook playbook.yml

## Notes

- Be sure to modify the `zabbix_hostgroup` task to match your desired host group in the Zabbix setup.
- Use Ansible Vault or environment variables to securely manage sensitive information like credentials and server IP addresses.
