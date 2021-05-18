# ocp4-mirror-registry

The the goal of this project is to deploy two types registries to support OCP installation.

1. Pull-through caching registry 
2. Mirror registry 

## Pull-through caching registry

This will create a podman docker registry for each of the following OpenShift registry URLs:
   - https://cloud.openshift.com
   - https://cdn02.quay.io
   - https://quay.io'
   - https://registry.connect.redhat.com
   - https://registry.svc.ci.openshift.org
   - https://registry.redhat.io


### Steps to get started

After cloning this project.

* Generate the registries vars file

```
bash scripts/generate-registry-proxies.sh your-openshift-pull-secret > playbooks/vars/registries.yml
```

* Review the vars file

`playbooks/roles/ocp4-mirror-registry/vars/main.yml`

You can edit that file directly or add your overrides to `playbooks/vars/registries.yml`.

* Run the ansible playbook

```
ansible-playbook playbooks/deploy-mirror-registry.yml
```

## Mirror registry