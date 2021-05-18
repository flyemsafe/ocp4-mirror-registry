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

**Generate the registries vars file**

```
bash scripts/generate-registry-proxies.sh your-openshift-pull-secret > playbooks/vars/registries.yml
```

**Review the vars file**

You might want to change the following vars:
 - registry_domain
 - registry_ip
 - pull_secret
 - generated_pull_secret
 - additional_trust_bundle
 - image_content_sources
 - container_user
 - container_group
 - self_signed_certs_options
 - registry_username
 - registry_password

`playbooks/roles/ocp4-mirror-registry/vars/main.yml`

You can edit that file directly or add your overrides to `playbooks/vars/registries.yml`.

**Run the ansible playbook**

```
ansible-playbook playbooks/deploy-mirror-registry.yml
```

**End result**

* Podman containers running
```
sudo podman ps -a | grep reg
f2a39547c388  docker.io/library/registry:2.7.1                       /etc/docker/regis...  17 minutes ago  Up 17 minutes ago  0.0.0.0:5003->5003/tcp  reg-redhat
75798cc3e518  docker.io/library/registry:2.7.1                       /etc/docker/regis...  18 minutes ago  Up 17 minutes ago  0.0.0.0:5005->5005/tcp  reg-svc
31b877250f63  docker.io/library/registry:2.7.1                       /etc/docker/regis...  18 minutes ago  Up 18 minutes ago  0.0.0.0:5002->5002/tcp  reg-connect-rh
f21393be9dd3  docker.io/library/registry:2.7.1                       /etc/docker/regis...  18 minutes ago  Up 18 minutes ago  0.0.0.0:5001->5001/tcp  reg-quay
65f212fb80b6  docker.io/library/registry:2.7.1                       /etc/docker/regis...  18 minutes ago  Up 18 minutes ago  0.0.0.0:5004->5004/tcp  reg-quay-cdn
204899041d74  docker.io/library/registry:2.7.1                       /etc/docker/regis...  18 minutes ago  Up 18 minutes ago  0.0.0.0:5000->5000/tcp  reg-cloud-ocp
```
Files are created to use with the openshift install to add to your `install-config.yaml`
- generated_pull_secret
- additional_trust_bundle
- image_content_sources

Example snippet
```
pullSecret: '{
  "auths": {
    "registry.lab.qubinode.io:5000": {
      "auth": "cmVnYWRtaW46cmVnYWRtaW4="
    },
....
additionalTrustBundle: |
  -----BEGIN CERTIFICATE-----
  MIIEZTCCAk0CFC9udoXKu7f2vwM+S9Ke1HxEUdzmMA0GCSqGSIb3DQEBCwUAMG8x
  CzAJBgNVBAYTAlVTMQswCQYDVQQIDAJGTDEPMA0GA1UEBwwGT3JhbmdlMREwDwYD
....
imageContentSources:
- mirrors: 
  - registry.lab.qubinode.io:5000/openshift-release-dev/ocp-release
  source: quay.io/openshift-release-dev/ocp-releas
...
```

## Mirror registry