# ocp4-mirror-registry
Role to stand a OCP4 mirror registry

## Requirements

A RHEL 8 host that's registered to Red Hat Subscription Management (RHSM) or Red Hat Satellite.

## Usage
* Clone or download the zip file of the project to your ansible controller
* Default option is to generate self-signed certs.
   * Edit the var `self_signed_certs_options` in playbooks/vars/main.yml
* To provide your own certs:
  * Edit the var `generate_self_signed_certs` in playbooks/vars/main.yml
  * Edit the var `ssl_cert_file` and `ssl_key_file` in playbooks/vars/main.yml
* Download the required Ansible Collections with: ```ansible-galaxy collection install -r playbooks/requirements.yml```
* Run the playbook: ```ansible-playbook playbooks/create_registry.yml -i inventory/```
