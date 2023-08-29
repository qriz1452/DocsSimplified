# Getting started 
Link : https://prometheus.io/docs/prometheus/latest/getting_started/

---------------------------



* Downloading and running Prometheus via wget : https://prometheus.io/download
```
tar xvfz prometheus-*.tar.gz
cd prometheus-*
```

* Configuring Prometheus to monitor itself :
  -  collects metrics from targets by scraping metrics HTTP endpoints
  -  it can also scrape and monitor its own health.
  -  example prometheus.yml file :
 ```
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:9090']
```

* Starting Prometheus :
   - at localhost:9090
   - can access metrics on endpoint: localhost:9090/metrics

    
* Using the expression browser :
  - navigate to http://localhost:9090/graph and choose the "Table" view within the "Graph" tab.
  - one metric that Prometheus exports about itself is named `prometheus_target_interval_length_seconds`
  - If we are interested only in 99th percentile latencies, we could use this query: `prometheus_target_interval_length_seconds{quantile="0.99"}`
  - To count the number of returned time series, you could write: `count(prometheus_target_interval_length_seconds)`
  


// few imp points i got :
1. cAdvisor. In the same way the Node exporter provides metrics about the machine, cAdvisor is an exporter that provides metrics about cgroups. Cgroups are a Linux kernel isolation feature that are usually used to implement containers on Linux, and are also used by runtime environments such as systemd.
2. The main difference between a Telegraf + InfluxDB setup and Prometheus + Node Exporter is the exporter part. We could differentiate these as push mode and poll mode. Basically, Telegraf will push the metrics to InfluxDB while Prometheus will poll the data from Node exporter. This will greatly impact your network setup
3. A Node Exporter is needed on all servers or virtual machines to collect data on all nodes; Node Exporter exposes metrics on '/metrics' sub-path on port 9100.





* Using the graphing interface :
   -  navigate to http://localhost:9090/graph and use the "Graph" tab
   -  `rate(prometheus_tsdb_head_chunks_created_total[1m])`

* Starting up some sample targets :
  - here  Node Exporter is used as an example target

```
tar -xzvf node_exporter-*.*.tar.gz
cd node_exporter-*.*

# Start 3 example targets in separate terminals:
./node_exporter --web.listen-address 127.0.0.1:8080
./node_exporter --web.listen-address 127.0.0.1:8081
./node_exporter --web.listen-address 127.0.0.1:8082
```
  - You should now have example targets listening on http://localhost:8080/metrics, http://localhost:8081/metrics, and http://localhost:8082/metrics.
    

* Configure Prometheus to monitor the sample targets :
  - group all three endpoints into one job called node , 2 are prod server . 1  is canary
  - we will add the group="production" label to the first group of targets, while adding group="canary"
  - so our peometheus.yml file looks like this: 
```
scrape_configs:
  - job_name:       'node'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:8080', 'localhost:8081']
        labels:
          group: 'production'

      - targets: ['localhost:8082']
        labels:
          group: 'canary'


```
  - Go to the expression browser and verify that Prometheus now has information about time series that these example endpoints expose, such as `node_cpu_seconds_total`

 
* Configure rules for aggregating scraped data into new time series  :
  -  queries that aggregate over thousands of time series can get slow when computed ad-hoc. To make this more efficient,  ` avg by (job, instance, mode) (rate(node_cpu_seconds_total[5m]))`
  -   we can create a new metrics to record these metric ,  create a file with the following recording rule and save it as prometheus.rules.yml
  
```
groups:
- name: cpu-node
  rules:
  - record: job_instance_mode:node_cpu_seconds:avg_rate5m
    expr: avg by (job, instance, mode) (rate(node_cpu_seconds_total[5m]))
```
  - add a rule_files statement in your prometheus.yml
```
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.
  evaluation_interval: 15s # Evaluate rules every 15 seconds.

  # Attach these extra labels to all timeseries collected by this Prometheus instance.
  external_labels:
    monitor: 'codelab-monitor'

rule_files:
  - 'prometheus.rules.yml'

scrape_configs:
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:9090']

  - job_name:       'node'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:8080', 'localhost:8081']
        labels:
          group: 'production'

      - targets: ['localhost:8082']
        labels:
          group: 'canary'

```
 - Restart Prometheus with the new configuration and verify that a new time series with the metric name `job_instance_mode:node_cpu_seconds:avg_rate5m` is now available by querying it through the expression browser or graphing it.

* Reloading configuration :
  - configuration reloaded without restarting  using `kill -s SIGHUP <PID>`

* Shutting down your instance gracefully. :
  -  Prometheus does have recovery mechanisms in the case that there is an abrupt process failure it is recommend to use the SIGTERM signal to cleanly shutdown a Prometheus instance usimg `kill -s SIGTERM <PID>`
    


=====================================================================


# Installation
https://prometheus.io/docs/prometheus/latest/installation/#installation

------------------------------------


* Using pre-compiled binaries : 
* From source
* Using Docker
  - can provide our on configurtions lie volume and bind mount ,port , custom image copying yaml file to prom container, etc
  - Bind mount  your prometheus.yml from the host by running:

```
docker run \
    -p 9090:9090 \
    -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
    prom/prometheus

```
  - bind-mount the directory containing prometheus.yml onto /etc/prometheus by running:
```

docker run \
    -p 9090:9090 \
    -v /path/to/config:/etc/prometheus \
    prom/prometheus

```
* Using configuration management systems :
  - Ansible
  - Chef
  - Puppet
  - SaltStack
 


===========================================================

# Configuration :
https://prometheus.io/docs/prometheus/latest/configuration/configuration/#configuration

--------------------

* To view all available command-line flags, run ./prometheus -h
* If the new configuration is not well-formed, the changes will not be applied
*  A configuration reload is triggered by sending a `SIGHUP` to the Prometheus process or sending a `HTTP POST` request to the `/-/reload` endpoint .

* Configuration file :
  -  To specify which configuration file to load, use the --config.file flag
  -   YAML format,
  -    Brackets indicate that a parameter is optional.
  -    example file : https://github.com/prometheus/prometheus/blob/release-2.46/config/testdata/conf.good.yml

* <scrape_config> :
  - A scrape_config section specifies a set of targets and parameters describing how to scrape them.
  - Targets may be statically configured via the static_configs parameter or dynamically discovered using one of the supported service-discovery mechanisms..
  - relabel_configs allow advanced modifications to any target and its labels before scraping.

* <tls_config> :
  - A tls_config allows configuring TLS connections.

*  <oauth2>  :
  - OAuth 2.0 authentication using the client credentials grant type. Prometheus fetches an access token from the specified endpoint with the given client access and secret keys.

    
* <azure_sd_config> :
  - Azure SD configurations allow retrieving scrape targets from Azure VMs.

*  <consul_sd_config> :
  - Consul SD configurations allow retrieving scrape targets from Consul's Catalog API.

*  <digitalocean_sd_config>  :
  - DigitalOcean SD configurations allow retrieving scrape targets from DigitalOcean's Droplets API.

*  <docker_sd_config>  :
  - Docker SD configurations allow retrieving scrape targets from Docker Engine hosts.
  - This SD discovers "containers" and will create a target for each network IP and port the container is configured to expose.

*  <dockerswarm_sd_config>  :
  - Docker Swarm SD configurations allow retrieving scrape targets from Docker Swarm engine.
  - more options available service , tasks, nodes ,

* More available here :
  
    Configuration file
        <scrape_config>
        <tls_config>
        <oauth2>
        <azure_sd_config>
        <consul_sd_config>
        <digitalocean_sd_config>
        <docker_sd_config>
        <dockerswarm_sd_config>
        <dns_sd_config>
        <ec2_sd_config>
        <openstack_sd_config>
        <ovhcloud_sd_config>
        <puppetdb_sd_config>
        <file_sd_config>
        <gce_sd_config>
        <hetzner_sd_config>
        <http_sd_config>
        <ionos_sd_config>
        <kubernetes_sd_config>
        <kuma_sd_config>
        <lightsail_sd_config>
        <linode_sd_config>
        <marathon_sd_config>
        <nerve_sd_config>
        <nomad_sd_config>
        <serverset_sd_config>
        <triton_sd_config>
        <eureka_sd_config>
        <scaleway_sd_config>
        <uyuni_sd_config>
        <vultr_sd_config>
        <static_config>
        <relabel_config>
        <metric_relabel_configs>
        <alert_relabel_configs>
        <alertmanager_config>
        <remote_write>
        <remote_read>
        <tsdb>
        <exemplars>
        <tracing_config>


* VERY LONG ARTICLE PLEASE REFER ORIGINAL DOCS.
  
==============================================================

## Defining recording rules :
https://prometheus.io/docs/prometheus/latest/configuration/recording_rules/#defining-recording-rules

----------------------------------

* Configuring rules  :
  - supports two types of rules recording rules and alerting rules
  - To include rules in Prometheus, create a file containing the necessary rule statements and have Prometheus load the file via the rule_files field in the Prometheus configuration. Rule files use YAML.
  -  can be reloaded at runtime by sending SIGHUP
  -  changes are only applied if all rule files are well-formatted.


* Syntax-checking rules :
  -  check whether a rule file is syntactically correct without starting a Prometheus server, you can use Prometheus's promtool command-line utility tool: ` promtool check rules /path/to/example.rules.yml `
  -  When the file is syntactically valid, the checker prints a textual representation of the parsed rules to standard output and then exits with a 0 return status.
  -  If there are any syntax errors or invalid input arguments, it prints an error message to standard error and exits with a 1 return status.

* Recording rules :
  -   allow you to precompute frequently needed or computationally expensive expressions and save their result as a new set of time series.
  - result will then often be much faster than executing the original expression
  - especially useful for dashboards,
  - Recording and alerting rules exist in a rule group. Rules within a group are run sequentially at a regular interval, with the same evaluation time. The names of recording rules must be valid metric names. The names of alerting rules must be valid label values.
```
groups:
  - name: example
    rules:
    - record: code:prometheus_http_requests_total:sum
      expr: sum by (code) (prometheus_http_requests_total)

```
  - The syntax for alerting rules is:
    
```
# The name of the alert. Must be a valid label value.
alert: <string>

# The PromQL expression to evaluate. Every evaluation cycle this is
# evaluated at the current time, and all resultant time series become
# pending/firing alerts.
expr: <string>

# Alerts are considered firing once they have been returned for this long.
# Alerts which have not yet fired for long enough are considered pending.
[ for: <duration> | default = 0s ]

# How long an alert will continue firing after the condition that triggered it
# has cleared.
[ keep_firing_for: <duration> | default = 0s ]

# Labels to add or overwrite for each alert.
labels:
  [ <labelname>: <tmpl_string> ]

# Annotations to add to each alert.
annotations:
  [ <labelname>: <tmpl_string> ]
```
















