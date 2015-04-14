aspects_logstash_forwarder
=========

Install and configure pre-compiled instances of logstash-forwarder.

Requirements
------------

Set ```hash_behaviour=merge``` in your ansible.cfg file.

If running on Suse, you will need to manage starting and stopping logstash-forwarder yourself. The init script rpm deploys does not work.

I used Monit and an updated version of start-stop-daemon to get it running.

Role Variables
--------------

* aspects_logstash_forwarder_enabled
* aspects_logstash_forwarder_run_install
* aspects_logstash_forwarder_package_name_Debian
* aspects_logstash_forwarder_package_name_RedHat
* aspects_logstash_forwarder_package_name_Suse
* aspects_logstash_forwarder_package_remote_directory
* aspects_logstash_forwarder_cert_key
* aspects_logstash_forwarder_cert_crt
* aspects_logstash_forwarder_cert_ca
* aspects_logstash_forwarder_servers
* aspects_logstash_forwarder_timeout
* aspects_logstash_forwarder_inputs

See ```defaults/main.yml``` for description and examples.


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      vars:
        aspects_logstash_forwarder_run_install: true
        aspects_logstash_forwarder_package_name_Debian: "logstash-forwarder_0.3.1_amd64.deb"
        aspects_logstash_forwarder_package_remote_directory: /root
        aspects_logstash_forwarder_cert_key: "/path/to/ssl/private/key"
        aspects_logstash_forwarder_cert_crt: "/path/to/ssl/main/cert"
        aspects_logstash_forwarder_cert_ca: "/path/to/ssl/cert/authority"
        aspects_logstash_forwarder_servers:
          exampleserver:
            use: true
            hostname: localhost
            port: 5043
        aspects_logstash_forwarder_inputs:
          apache2: |
            {
              "paths": [
                "/var/log/apache2/*.log",
                "/var/log/httpd/*.log"
              ],
              "fields": { "type": "syslog" }
            }
          syslog: |
            {
              "paths": [
                "/var/log/messages",
                "/var/log/syslog"
              ],
              "fields": { "type": "syslog" }
            }
      roles:
         - { role: aspects_logstash_forwarder_enabled, when: aspects_logstash_forwarder_enabled }

License
-------

MIT