# Ansible Openresty

[![galaxy](https://img.shields.io/badge/galaxy-quantmind.openresty-blue.svg)](https://galaxy.ansible.com/quantmind/openresty/)
[![Build Status](https://travis-ci.org/quantmind/ansible-openresty.svg?branch=master)](https://travis-ci.org/quantmind/ansible-openresty)

Ansible role to create an openresty docker images and add configuration files.

## Create Image

To create an image the ``openresty_create_image`` must be set to ``true`` (default is ``false``).
An example playbook:
```yaml
- name: Create the docker image for openresty

  hosts: nginx

  vars:
    openresty_create_image: true

  roles:
    - quantmind.openresty

```

## Install sites

When ``openresty_create_image`` is set to ``false`` (the default value), the role installs

* Nginx configuration files for web sites in ``openresty_volume_nginx_config_path``
* SSL server certificates in ``openresty_volume_nginx_ssl_path``
* Html/static assets in ``openresty_volume_nginx_html_path``

### Configuration files

These are defined in the ``services`` list. The list containes objects with
schema::
```json
{
}
```

