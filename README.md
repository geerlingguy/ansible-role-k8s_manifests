# Ansible Role: K8s Manifests

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-k8s_manifests.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-k8s_manifests)

An Ansible Role that applies Kubernetes manifests (either templated, or directly) to Kubernetes clusters.

## Requirements

  - Pip package: `openshift`

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    k8s_manifests:
      - monitoring/prometheus
      - dir: monitoring/grafana-configmap
        lookup_type: 'file'

A list of Kubernetes manifest directories to apply to a Kubernetes cluster. This list can either be raw directory paths or folder names, or can be a dict with `dir` (the directory path/folder name) and `lookup_type` (the Ansible lookup type used for the `manifest.yml` file).

This role then looks inside the specified directory for each manifest, and applies a `manifest.yml` file (and all it's contents) using the Ansible `k8s` module.

If you need to template the file, this role templates the `manifest.yml` file by default (and automatically adds any variables in a `vars.yml` file alongside the `manifest.yml` file). But you can also disable templating and have the manifest applied directly by setting `lookup_type: file`.

    k8s_manifests_base_dir: ''

If set, this string will be prepended to each manifest `dir`/path specified in `k8s_manifests`. This is useful if you store all your Kubernetes manifests in a directory outside the Ansible playbook directory, so you don't have to include the full path in each defined `k8s_manifests` list item.

    k8s_manifests_state: present

Whether the `state` for the `k8s` module should be `present` or `absent`. Note that using `absent` doesn't _always_ delete all Kubernetes resources defined in a manifest.

    k8s_kubeconfig: ~/.kube/config

The path to the `kubeconfig` file to use to connect to the Kubernetes cluster.

    k8s_resource_namespace: ''
    k8s_manage_namespace: true

By default, this role assumes you will be deploying resources into a particular namespace. So if you set `k8s_resource_namespace` to the namespace you are operating on, the role will ensure that namespace exists before applying any manifests. You can disable this role's namespace management (e.g. if the manifest you're applying should not be namespaced) by setting `k8s_manage_namespace: false`.

    k8s_no_log: true

Whether to log the details of each manifest's application to the cluster in the Ansible output. Secrets and other sensitive data could be part of a manifest, so this is set to be secure by default. Set to `false` for debugging purposes.

## Dependencies

None.

## Example Playbook

    ---
    - hosts: localhost
      connection: local
      gather_facts: no
    
      vars:
        k8s_kubeconfig: ~/.kube/config-my-cluster
        k8s_manifests_base_dir: k8s-manifests/
        k8s_manifests:
          - storageclass
    
      roles:
        - role: geerlingguy.k8s_manifests

See the `k8s-manifests` directory and it's README for an example templated manifest layout with a vars file defined alongside it.

## License

MIT / BSD

## Author Information

This role was created in 2018 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
