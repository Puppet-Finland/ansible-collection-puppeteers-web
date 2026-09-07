Reverse Proxy
=========

This role manages a basic reverse proxy for different servies while enforcing HTTPS. This role also supports an optional basic auth setup as well for each service.

Role Variables
--------------
Required variables are as follows:

- puppeteers_web_reverse_proxy_services
- puppeteers_web_reverse_proxy_base_domain
- puppeteers_web_reverse_proxy_fullchain_content
- puppeteers_web_reverse_proxy_fullchain_key_content

Example Hostvars:

```yaml
    puppeteers_web_reverse_proxy_services:
        - name: prometheus
          port: 9090
        - name: alertmanager
          port: 9093
          basic_auth_users:
            - name: monit
              password: {{ vault_alertmanager_monit_basic_auth_secret }}

    puppeteers_web_reverse_proxy_base_domain: beta.example.com

    puppeteers_web_reverse_proxy_fullchain_content: {{ Fullchain_Certificate_content }}

    puppeteers_web_reverse_proxy_fullchain_key_content: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          --- snip --
```

The parameter **puppeteers_web_reverse_proxy_base_domain** is appended to each service name within
the vhost config setting:

```config
    # /etc/nginx/conf.d/prometheus.conf
    server {
        listen 443 ssl;
        server_name prometheus.beta.example.com;
        --- snip ---
```

License
-------
BSD-2-Clause
