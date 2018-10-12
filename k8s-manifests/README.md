# K8s Manifest Example Directory

The 'storageclass' example in this directory illustrates the structure of Kubernetes manifest files as consumed by the `geerlingguy.k8s_manifests` role.

To apply the `storageclass` manifest, you would call this role with the variable:

    k8s_manifests:
      - k8s-manifests/storageclass

When the role runs, it will template the `manifest.yml` file inside the manifest directory, and apply any variables defined in an optional `vars.yml` file alongside the manifest (along with any other variables the Ansible playbook has already loaded, e.g. from inventory or dynamically).

You can also apply a manifest directly, with no templating, using:

    k8s_manifests:
      - dir: k8s-manifests/storageclass
        lookup_type: file

This can be useful for raw manifests which do not need any templating, or manifest files which contain Jinja-like syntax which is hard to escape properly.
