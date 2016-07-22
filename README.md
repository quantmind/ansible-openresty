# Ansible Openresty

[![galaxy](https://img.shields.io/badge/galaxy-quantmind.openresty-blue.svg)](https://galaxy.ansible.com/quantmind/openresty/)
[![Build Status](https://travis-ci.org/quantmind/ansible-openresty.svg?branch=master)](https://travis-ci.org/quantmind/ansible-openresty)

**Docker repository**: [quantmind/openresty](https://hub.docker.com/r/quantmind/openresty/)

Ansible role to create [openresty][] docker images and add configuration files.

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

When an image is createdm the ``nginx.conf`` file is also populated and the following
variable are used.

**nginx_maps**

Add [nginx map](http://nginx.org/en/docs/http/ngx_http_map_module.html) directives to the configuration.
A list of objects with schema:
```json
{
    "from": "<mapping from>",
    "to": "<mapping to>",
    "entries": [
        "<line 1>",
        ...
    ]
}
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

[openresty]: https://openresty.org/en/
