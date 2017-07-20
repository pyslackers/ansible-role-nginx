NGINX
=========

[![Build Status](https://travis-ci.org/pyslackers/ansible-role-nginx.svg?branch=master)](https://travis-ci.org/pyslackers/ansible-role-nginx)

Role for NGINX that assumes serving a long running server process, with support for automatic SSL certificate provisioning and renewal from Let's Encrypt.

Requirements
------------

None

Role Variables
--------------

The role takes a dictionary of variables:

```yaml
sites:  # dict of sites to provision
  sirbot:
    # list of domains for this site to respond do, include `www.` and non-`www.`
    # to ensure all are handled, required
    domains:
      - sirbot.pyslackers.com
    port: 5000  # local port the application will listen on, required
    # Whether or not to provision SSL, default: false.
    # note: for this to work, your server must be accessible to the interweb
    ssl: false  # whether or not to provision SSL, 
    static: ''  # optional static directory to serve as well
    email: noreply@example.com  # who should be emailed on cert expiration, if our auto-renewal fails. Required if ssl: true
```

Dependencies
------------

None

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
    - role: pyslackers.nginx
      sites:
        pyslackers:
          domains:
            - www.pyslackers.com
            - pyslackers.com
          port: 8000
          ssl: true
          static: /var/www/pyslackers/static/
          email: pythondev.slack@gmail.com
        sirbot:
          domains:
            - sirbot.pyslackers.com
          port: 8080
          ssl: true
          email: pythondev.slack@gmail.com
```

License
-------

MIT

Author Information
------------------

None
