This playbook is a slim wrapper around Jeff Geerling's [apache role](https://github.com/geerlingguy/ansible-role-apache).

The default vars, defined in [all.yml](group_vars/all.yml), are what's needed to configure a proxy for two backend servers using self-signed SSL. Other configurations are certainly possible.

## Options

This playbook allows you to define one or more sudo users and to configure SSL with either a self-signed cert or one from CA.

**proxy_users:** is list of user objects (name, public key filepath) that will be created and granted sudo via membership in the `wheel` group, which is also created by this playbook.

~~~
proxy_users:
  - name: superadmin
    pubkeyfile: files/superadmin.pub
~~~

**self_sign:** whether to create self-signed certificate (default) or to use files provided by a CA (`self_sign: no`). NB: that the self-signed cert produced by this playbook does not include an intermediate/chain/bundle cert, and the variable `proxy_ssl_use_chain: no` prevents the corresponding directive from being included in the apache vhost declaration.


**SSL paths:** The following control where SSL files go. **TODO** the real-life certificates, for which this playbook is written to deploy, specify multiple names, and so the same cert is used for multiple vhosts.


    proxy_cert_filename: examplec.com.crt
    proxy_cert_path: /etc/ssl/certs
    proxy_chain_filename: examplec.com_chain.crt
    proxy_key_filename: examplec.com.key
    proxy_key_path: /etc/ssl/private

**proxy details:** these variables are consumed in the `apache_vhosts` and `apache_vhosts_ssl` variables:

    proxy_production_backend_ip: 192.168.111.1
    proxy_test_backend_ip: 192.168.111.2
    proxy_apache_email: me@example.com
    proxy_production_hostname: prod.example.com
    proxy_test_hostname: test.example.com
