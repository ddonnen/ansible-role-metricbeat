# Ansible Role: Metricbeat for ELK Stack

An Ansible Role that installs [Metricbeat](https://www.elastic.co/products/beats/metricbeat) on RedHat/CentOS or Debian/Ubuntu.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    metricbeat_create_config: true

Whether to create the Metricbeat configuration file and handle the copying of SSL key and cert for metricbeat. If you prefer to create a configuration file yourself you can set this to `false`.

    metricbeat_prospectors:
      - input_type: log
        paths:
          - "/var/log/*.log"

Prospectors that will be listed in the `prospectors` section of the Metricbeat configuration. Read through the [Metricbeat Prospectors configuration guide](https://www.elastic.co/guide/en/beats/metricbeat/current/configuration-metricbeat-options.html) for more options.

    metricbeat_output_elasticsearch_enabled: false
    metricbeat_output_elasticsearch_hosts:
      - "localhost:9200"

Whether to enable Elasticsearch output, and which hosts to send output to.

    metricbeat_output_logstash_enabled: true
    metricbeat_output_logstash_hosts:
      - "localhost:5000"

Whether to enable Logstash output, and which hosts to send output to.

    metricbeat_enable_logging: false 
    metricbeat_log_level: warning
    metricbeat_log_dir: /var/log/metricbeat
    metricbeat_log_filename: metricbeat.log

Metricbeat logging.

    metricbeat_ssl_dir: /etc/pki/logstash

The path where certificates and keyfiles will be stored.

    metricbeat_ssl_certificate_file: ""
    metricbeat_ssl_key_file: ""

Local paths to the SSL certificate and key files, which will be copied into the `metricbeat_ssl_dir`.

For utmost security, you should use your own valid certificate and keyfile, and update the `metricbeat_ssl_*` variables in your playbook to use your certificate.

To generate a self-signed certificate/key pair, you can use use the command:

    $ sudo openssl req -x509 -batch -nodes -days 3650 -newkey rsa:2048 -keyout metricbeat.key -out metricbeat.crt

Note that metricbeat and logstash may not work correctly with self-signed certificates unless you also have the full chain of trust (including the Certificate Authority for your self-signed cert) added on your server. See: https://github.com/elastic/logstash/issues/4926#issuecomment-203936891

    metricbeat_ssl_insecure: "false"

Set this to `"true"` to allow the use of self-signed certificates (when a CA isn't available).

## Dependencies

None.

## Example Playbook

    - hosts: logs
      roles:
        - ddonnen.metricbeat

## License

MIT / BSD

## Author Information

This role was created in 2016 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
