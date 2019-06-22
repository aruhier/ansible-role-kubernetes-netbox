Ansible Role: Netbox for Kubernetes
===================================

Ansible role to install Netbox on Kubernetes.

Role Variables
--------------

```yaml
# Namespace
kubernetes_netbox_namespace: "default"
# App name (used as selector)
kubernetes_netbox_app: "netbox"
# Deployment name
kubernetes_netbox_deployment: "netbox-deployment"
# Configmap name
kubernetes_netbox_configmap: "netbox"
# Secret name
kubernetes_netbox_secret: "netbox"
# Service name
kubernetes_netbox_service: "netbox"

# Number of replicas
kubernetes_netbox_replicas: 1
kubernetes_netbox_revision_history: 1

# Node selector
kubernetes_netbox_node_selector: {}

# Add custom labels in the deployment metadata section
kubernetes_netbox_deployment_labels: {}
# Add custom annotations in the deployment metadata section
kubernetes_netbox_deployment_annotations: {}

kubernetes_netbox_resources:
  limits:
    memory: "512Mi"
  requests:
    memory: "128Mi"

kubernetes_netbox_nginx_resources:
  limits:
    memory: "256Mi"
  requests:
    memory: "64Mi"

# Setup ingress for netbox
kubernetes_netbox_setup_ingress: false
kubernetes_netbox_ingress:
  name: "netbox-ingress"
  host: "netbox.example.com"
  annotations:
  tls:

### Netbox options ###

# Netbox secret key. CHANGE IT
kubernetes_netbox_secret_key: ""

# CORS origin whitelist, space separated. MANDATORY
kubernetes_netbox_cors_origin_whitelist:
# example:
# kubernetes_netbox_cors_origin_whitelist: "https://example.com http://example.net"

# Netbox database
kubernetes_netbox_db:
  host: "db.example.com"
  port:
  name: "netbox"
  user: "netbox"
  password: "netbox"

# Email options
kubernetes_netbox_email_from: "netbox@example.com"
kubernetes_netbox_email_server: "mail.example.com"
kubernetes_netbox_email_password: "netbox"
kubernetes_netbox_email_user: "netbox"
kubernetes_netbox_email_port: 25
kubernetes_netbox_email_timeout: 10

kubernetes_netbox_login_required: true
kubernetes_netbox_su_email: "su@example.com"
kubernetes_netbox_su_name: "superuser"
kubernetes_netbox_su_password: "superuser"

kubernetes_netbox_napalm_user: ""
kubernetes_netbox_napalm_password: ""
```

Dependencies
------------

Kubectl needs to be installed on the host targeted by the role.


Example Playbook
----------------

```yaml

- hosts: kube-master
  run_once: true
  vars:
    kubernetes_netbox_secret_key: "a_not_so_random_secret_key"

    kubernetes_netbox_cors_origin_whitelist: ["example.com"]

    kubernetes_netbox_db:
      host: "db.example.com"
      name: "netbox"
      user: "netbox"
      password: "netbox"

    kubernetes_netbox_setup_ingress: true
    kubernetes_netbox_ingress:
      name: "netbox-ingress"
      host: "netbox.example.com"
      annotations:
        kubernetes.io/tls-acme: "true"
      tls:
        - secretName: "netbox-ingress-tls"
          hosts:
            - "netbox.example.com"
  roles:
    - role: Anthony25.kubernetes-netbox
```

Use `run_once` to run the role on only one available master in the cluster.

License
-------

Tool under the BSD license. Do not hesitate to report bugs, ask me some
questions or do some pull request if you want to!
