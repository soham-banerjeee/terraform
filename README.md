
```markdown
# Ansible Playbook Execution Guide

This guide provides instructions on how to run an Ansible playbook against an inventory, limit the run to a particular server, and execute the playbook with extra variables.

## Prerequisites

1. **Ansible Installation**: Ensure Ansible is installed on your control node (the machine from which you run Ansible). Install Ansible using the following commands:

   ```sh
   sudo apt-get install ansible
   ```

   Alternatively, using `pip`:

   ```sh
   pip install ansible
   ```

2. **Inventory File**: Ensure you have an inventory file. The inventory file can be in INI or YAML format. Here's an example of an INI format inventory file (`inventory.ini`):

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

3. **Playbook**: Ensure you have an Ansible playbook. Here's an example playbook (`site.yml`):

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

This command runs the `site.yml` playbook against all the hosts defined in the `inventory.ini` file.

### 2. Limit the Run Against a Particular/Single Server

To limit the execution of the playbook to a particular server, use the `--limit` option:

```sh
ansible-playbook -i inventory.ini site.yml --limit prod-bastion-01
```

This command runs the `site.yml` playbook only against the `prod-bastion-01` server defined in the `inventory.ini` file.

### 3. Run the Playbook with Extra Variables

To pass extra variables to the playbook at runtime, use the `--extra-vars` (or `-e`) option:

```sh
ansible-playbook -i inventory.ini site.yml -e "var1=value1 var2=value2"
```

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

This command runs the `site.yml` playbook against the `prod-bastion-01` server, passing the `http_port` and `max_clients` variables.

## Summary

- **Run against the whole inventory group**: `ansible-playbook -i inventory.ini site.yml`
- **Limit to a particular server**: `ansible-playbook -i inventory.ini site.yml --limit <hostname>`
- **Run with extra variables**: `ansible-playbook -i inventory.ini site.yml -e "var1=value1 var2=value2"`

These steps should help you manage running your Ansible playbooks against various inventories and configurations effectively.
```
