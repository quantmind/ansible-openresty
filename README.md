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

These are defined in the ``services`` list. The list contains objects with
schema::
```json
{
    "domain": "value of server_name in server directive",
    "certificate": "S3 key of SSL certificate",
    "locations": [
        {
            "location": "the location path or regex, required",
            "s3location": "Optional s3location",
            "host": "Optional host, used in proxy_pass directive",
            "port": "Optional port, required when host is used"
        },
        ...
    ]
}
```

* if ``domain`` is specified, a service entry creates a new configuration file
for nginx, otherwise it is ignored by this role.
* ``certificate`` when available the ``server`` is configured to listen on port 443 ofer a TLS connectin. The certificate is downloaded from the ``certificate_bucket``.

### location

When the ``location.host`` is available, the location is configured as ``proxy_pass`` and the ``location.port``
must be available for correct configuration.


[openresty]: https://openresty.org/en/
