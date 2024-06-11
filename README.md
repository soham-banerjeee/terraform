# Ansible Playbook Execution Guide

This guide provides instructions on how to run an Ansible playbook against an inventory, limit the run to a particular server, and execute the playbook with extra variables.

## Prerequisites

1. **Ansible Installation**: Ensure Ansible is installed on your control node (the machine from which you run Ansible). Install Ansible using the following commands:

   ```sh
   sudo apt-get install ansible

 Alternatively, using pip:
   ```sh
   pip install ansible

2. **Inventory File**: Ensure you have an inventory file. The inventory file can be in INI or YAML format. Here's an example of an INI format inventory file (inventory.ini):
