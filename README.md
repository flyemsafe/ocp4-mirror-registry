# ocp4-mirror-registry
Role to stand a OCP4 mirror registry

## Requirements

A RHEL 8 host that's registered to Red Hat Subscription Management (RHSM) or Red Hat Satellite.

## Usage
1. Clone the project to your ansible controller.
2. Edit the CSR options in playbooks/vars/main.yml
3. Download the required Ansible Collections with: ```ansible-galaxy collection install -r playbooks/requirements.yml```
4. Run the playbook: ```ansible-playbook playbooks/create_registry.yml```
