# Ansible Role: ECR Container Build

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-ecr-container-build.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-ecr-container-build)

An Ansible Role that installs builds Docker container images and (optionally) pushes them to AWS ECR Repositories.

## Requirements

  - Docker CE
  - Pip packages: `boto3`, `docker`

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    ecr_image_src_dir: ../my-project
    ecr_image_name: namespace/my-project

A source directory containing a Dockerfile and any required resources, and the image name (typically in the form `namespace/project`) for the docker image that is built.

    ecr_image_tags: ['latest']

TODO.

    ecr_push: true

Whether to push the built image to ECR. Set to `false` if you're just testing the image build portion or you cannot connect to ECR.

    ecr_region: us-east-1
    ecr_account_id: '123456789012'
    ecr_url: "{{ ecr_account_id }}.dkr.ecr.{{ ecr_region }}.amazonaws.com"

AWS account details for ECR.

## Dependencies

None.

## Example Playbook

Building locally (assuming you already have Docker CE and the `docker` pip package installed):

    ---
    - hosts: localhost
      connection: local
      gather_facts: false
    
      vars:
        ecr_image_src_dir: ../my-project
        ecr_image_name: namespace/my-project
        ecr_image_tags: ['latest','1.2.3']
        ecr_account_id: '123456789012'
        pip_install_packages: ['docker']

      roles:
        - role: geerlingguy.ecr-container-build


Building on a remote server:

    ---
    - hosts: localhost
      connection: local
      gather_facts: false
    
      vars:
        ecr_image_src_dir: ../my-project
        ecr_image_name: namespace/my-project
        ecr_image_tags: ['latest','1.2.3']
        ecr_account_id: '123456789012'
        pip_install_packages: ['docker']
    
      roles:
        - role: geerlingguy.docker
        - role: geerlingguy.pip
        - role: geerlingguy.ecr-container-build

## License

MIT / BSD

## Author Information

This role was created in 2018 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
