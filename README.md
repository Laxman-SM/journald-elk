# journald + elastic stack

Experimental logging stack:

journald --> journalbeat --> Kafka --> Logstash --> Elasticsearch --> Kibana

Getting started:
 * Have a VM with systemd and docker >= 1.12 (e.g.: centos/7 vagrant box)
 * Install python and systemd-python
 * Start self_test.py to generate test messages in the background
 * docker-compuse up -d

Debugging:
 * `journalctl -f --output json-pretty -t journal-selftest-heartbeat` : See the test messages in your journald on the node
 * `docker-compose logs -f journalbeat` : See Kafka connection errors, unacked' messages, etc.
 * `docker-compose exec logstash bash -c "tail -F /mnt/logstash/debug_current"` : To see the messages parsed by Logstash or take a look at the logstash logs: `docker-compose logs -f logstash`
 * `docker-compose logs -f elasticsearch` or see the indexed messages by running `curl -u elastic:changeme localhost:9200/logstash-YYYY-MM-dd/_search?pretty` 

Workflow:
 * Format messages as JSON, have at least a category field that will define the document type in ES
 * Edit the index mapping for logstash-* for your document type (https://opensource.appbase.io/gem/)
* Start logging

TODO:
 * Simple Python library for JSON logging + formatting for third party lib (for exceptions, there is Sentry as well which makes a lot more sense :))
