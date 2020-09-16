elb
=========

    Install and configure HA Proxy Load Balancer


Role Variables
--------------

    elb_install:            [default: true] install packages
    elb_haproxy_services:   [required] list of haproxy services
      - hostname:           [required] service hostname 
        redirect:           [optional] redirect rule for service
        allow_all:          [optional] allow all trafic without acl
        acl:                [optional] list of dict service acl rules
          - name:           [required] name of acl rule
            value:          [required] value of acl rule
        rules:              [not required] list of enable service acl rules
        backends:           [not required] list of backed servers
        headers:            [optional] custom service headers      
    elb_haproxy_certs:      [required] list of ssl pem


Example Playbook
----------------

    - hosts: elb
      vars: 
        elb_haproxy_certs:
          - '{{ role_path }}/../../certificates/wildcard.haproxy.pem'
          - '{{ role_path }}/../../certificates/app.haproxy.pem'
        elb_haproxy_services:
          - hostname: 'alias.domain.org'
            redirect: 'https://target.url.domain.org
          - hostname: 'app-1.domain.org'
            allow_all: 'true'
            backends:
              - 'app-1.domain.org:80'
            headers:
              - 'http-request  add-header X-Forwarded-Proto https'
          - hostname: 'app-2.domain.org'
            acl:
              - name: 'internal'
                value: 'src 192.168.0.0/24'
            rules:
              - 'internal'
            backends:
              - 'app-2.domain.org:80 ssl verify none'
  
      roles:
         - elb

License
-------

    MIT

Author Information
------------------

    Dmitrij Petrov
