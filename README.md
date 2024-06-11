
# Ansible Playbook Execution Guide

This guide provides detailed instructions on how to run an Ansible playbook against an inventory, limit the run to a particular server, and execute the playbook with extra variables.

## Prerequisites

### 1. Ansible Installation

Ensure Ansible is installed on your control node (the machine from which you run Ansible). You can install Ansible using the following commands:

#### Using `apt` (Debian-based systems):

```sh
sudo apt-get install ansible
```

#### Using `pip` (Python package installer):

```sh
pip install ansible
```

### 2. Inventory File

Ensure you have an inventory file. The inventory file can be in INI or YAML format. Here's an example of an INI format inventory file (`inventory.ini`):

```ini
[all:children]
production

[production:children]
bastion
elastic_agent
jenkinsci
logstash
qualys_agents
rundeck
webapps

[auditbeat:children]
production

[bastion]
prod-bastion-01
prod-bastion-01.us-central1-a.elastic-it-prod

[elastic_agent]
prod-bastion-01
prod-logstash-01
prod-rundeck-01
prod-webapp-02
prod-webapp-03

[logstash]
prod-logstash-01

[rundeck]
prod-rundeck-01

[webapps:children]
distro_list
roles_auditor

[distro_list]
prod-webapp-02

[jenkinsci]

[roles_auditor]
prod-webapp-03

[qualys_agents:children]
bastion
logstash
webapps
```

### 3. Playbook

Ensure you have an Ansible playbook. A playbook defines a series of tasks to be executed on the specified hosts. Here's an example playbook (`site.yml`):

```yaml
- name: Ensure web servers are configured
  hosts: webapps
  roles:
    - role: webserver
```

## Running the Playbook

### 1. Run Against Whole Inventory in the Group

To run the playbook against the entire group in the inventory, use the following command:

```sh
ansible-playbook -i inventory.ini site.yml
```

#### Explanation:

- `ansible-playbook`: This is the Ansible command used to run playbooks.
- `-i inventory.ini`: This option specifies the inventory file to use.
- `site.yml`: This is the playbook file that contains the tasks to execute.

This command will run the `site.yml` playbook against all the hosts defined in the `inventory.ini` file under the `webapps` group.

### 2. Limit the Run Against a Particular/Single Server

To limit the execution of the playbook to a particular server, use the `--limit` option:

```sh
ansible-playbook -i inventory.ini site.yml --limit prod-bastion-01
```

#### Explanation:

- `--limit prod-bastion-01`: This option limits the execution of the playbook to the `prod-bastion-01` server specified in the inventory file.

This command runs the `site.yml` playbook only against the `prod-bastion-01` server defined in the `inventory.ini` file.

### 3. Run the Playbook with Extra Variables

To pass extra variables to the playbook at runtime, use the `--extra-vars` (or `-e`) option:

```sh
ansible-playbook -i inventory.ini site.yml -e "var1=value1 var2=value2"
```

#### Explanation:

- `-e "var1=value1 var2=value2"`: This option allows you to pass additional variables to the playbook. Replace `var1=value1 var2=value2` with your actual variable names and values.

For example, if your playbook uses variables for configuration, you can pass them like this:

```sh
ansible-playbook -i inventory.ini site.yml -e "http_port=8080 max_clients=200"
```

This command passes the `http_port` and `max_clients` variables to the `site.yml` playbook.

### Putting It All Together

Here's an example command that combines all the options:

```sh
ansible-playbook -i inventory.ini site.yml --limit prod-bastion-01 -e "http_port=8080 max_clients=200"
```

#### Explanation:

- This command runs the `site.yml` playbook against the `prod-bastion-01` server, passing the `http_port` and `max_clients` variables.

## Summary

- **Run against the whole inventory group**:
  ```sh
  ansible-playbook -i inventory.ini site.yml
  ```

- **Limit to a particular server**:
  ```sh
  ansible-playbook -i inventory.ini site.yml --limit <hostname>
  ```

- **Run with extra variables**:
  ```sh
  ansible-playbook -i inventory.ini site.yml -e "var1=value1 var2=value2"
  ```

These steps should help you manage running your Ansible playbooks against various inventories and configurations effectively.
```
