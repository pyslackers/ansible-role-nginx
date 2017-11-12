NGINX
=========

[![Build Status](https://travis-ci.org/pyslackers/ansible-role-nginx.svg?branch=master)](https://travis-ci.org/pyslackers/ansible-role-nginx)

Ansible role to install and configure NGINX.

Role Variables
--------------

The role takes a dictionary of variables:

* `nginx_sites`: Dict of sites.
    * `domains`: List of domains serving this site.
    * `letsencrypt`: Use HTTPS and generate letsencrypt certificate for `domains` (use `staging` for the staging letsencrypt).
    * `locations`: Dict of custom nginx locations block.
        * `location`: Location match.
        * `static`: Configure location as static and use this folder.
        * `websocket`: Configure location for websocket.
        * `proxy_pass`: Proxy destination.
        * `custom`: Custom configuration for location.
        
* `certbot_port`: Listening port for certbot during certificates renewal (default to `32456`).

Dependencies
------------

* `pyslackers.letsencrypt`: Letsencrypt certificate generation.


Example Playbook
----------------

```yaml
- hosts: localhost
  vars:
    email: noreply@example.com
    nginx_sites:
      sirbot:
        domains:
          - sirbot.pyslackers.com
        locations:
          websock:
            location: /websocket
            websocket: true
            proxy_pass: http://127.0.0.1:5000
          custom:
            location: /custom
            custom: |
              proxy_set_header X-Hello-World hello;
            proxy_pass: http://127.0.0.1:5001
          static:
            location: /static
            static: /var/www/my_app/static/
          root:
            location: /
            proxy_pass: http://127.0.0.1:5000
  roles:
    - pyslackers.nginx
```

License
-------

MIT
