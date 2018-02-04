NGINX
=========

[![Build Status](https://travis-ci.org/pyslackers/ansible-role-nginx.svg?branch=master)](https://travis-ci.org/pyslackers/ansible-role-nginx)

Ansible role to install and configure NGINX.

Role Variables
--------------

The role takes a dictionary of variables:

* `nginx_sites`: Dict of sites.
    * `domains`: List of domains serving this site.
    * `template`: Jinja2 template used to generate site configuration (default to `etc/nginx/sites-available/site-available.j2`).
    * `letsencrypt`: Use HTTPS and generate letsencrypt certificate for `domains` (use `staging` for the staging letsencrypt).
    * `locations`: List of custom nginx locations block.
        * `location`: Location match.
        * `static`: Configure location as static and use this folder.
        * `websocket`: Configure location for websocket.
        * `proxy_pass`: Proxy destination.
        * `custom`: Custom configuration for location.
    * `gzip_enabled`: Whether or not to enable GZIP support (default: `true`)

* `nginx_auth`: Dict of sites where to enable basic-auth.
    * `site`: List of users for basic auth.
        * `user`: User
        * `password`: Password
        * `state`: State (default to `present`).

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
          - location: /websocket
            websocket: true
            proxy_pass: http://127.0.0.1:5000
          - location: /custom
            custom: |
              proxy_set_header X-Hello-World hello;
            proxy_pass: http://127.0.0.1:5001
          - location: /static
            static: /var/www/my_app/static/
          - location: /
            proxy_pass: http://127.0.0.1:5000
      pyslackers:
        domains:
          - pyslackers.com
        template: pyslackers.j2
  nginx_auth:
      pyslackers:
        - {name: foo, password: bar}
  roles:
    - pyslackers.nginx
```

License
-------

MIT
