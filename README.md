# Ansible Openresty

[![galaxy](https://img.shields.io/badge/galaxy-quantmind.openresty-blue.svg)](https://galaxy.ansible.com/quantmind/openresty/)
[![Build Status](https://travis-ci.org/quantmind/ansible-openresty.svg?branch=master)](https://travis-ci.org/quantmind/ansible-openresty)

**Docker repository**: [quantmind/openresty](https://hub.docker.com/r/quantmind/openresty/)

Ansible role to create [openresty][] docker images and install openresty
and configuration files on target machines.

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

When an image is created the ``nginx.conf`` file is also populated.


## Variables Only

To prevent installation, set ``openresty_install=false``. In this configuration
the role can be used only for the variables it specifies.

## Install sites

When ``openresty_create_image`` is set to ``false`` (the default value) and
``openresty_install`` is set to ``true`` (the default value)
the role installs

* Nginx configuration files for web sites in ``openresty_volume_nginx_config_path``
* SSL server certificates in ``openresty_volume_nginx_ssl_path``
* Html/static assets in ``openresty_volume_nginx_html_path``


### Configuration files

These are defined in the ``services`` list variable. The list contains objects with
schema:
```json
{
    "domain": "value of server_name in server directive",
    "certificate": "S3 key of SSL certificate",
    "lines": []
    "locations": [],
    "redirects": []
}
```

* If ``domain`` is specified, the service entry creates a new configuration file
  for an nginx [server block](http://nginx.org/en/docs/http/ngx_http_core_module.html#server),
  otherwise it is ignored by this role.
* When ``certificate`` is available the server block is configured to listen on port 443 over a TLS connection.
  The certificate is downloaded from the ``certificate_bucket`` and key given by ``certificate``.
* ``lines`` is an optional list of lines to add to the server block.
  Semicolon should be excluded form the line. For example::
  ```
        ["error_page 500 502 503 504 /custom_50x.html"]
  ```
  adds a configuration line for a custom template.
   
### location

The ``location`` object inside a ``service`` object has the following schema:
```json
{
    "location": "the location path or regex, required",
    "s3location": "Optional s3location",
    "host": "Optional host, used in proxy_pass directive",
    "port": "Optional port, required when host is used"
}
```

* When the **location.host** is available, the location is configured as ``proxy_pass`` and the ``location.port`` must be available for correct configuration.
* When the **location.lines** is available, the location is configured with the lines given (an array of string configuring the location).
* When the **location.s3location** is available, the location serves static assets from AWS S3.

### redirect

A redirect is an object in trhe ``servic.redirects`` list. As the name suggests, this entry adds a
redirect directive inside the server.
```json
{
    "from": "local path to redirect from",
    "to": "local path to redirect to",
    "filename": "Optional filename for the SSL certificate, if not provided, the from value is used"
}
```

[openresty]: https://openresty.org/en/
