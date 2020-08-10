# Description

This role configures [Cortex](https://cortexmetrics.io/) - a Horizontally scalable, highly available, multi-tenant, long term Prometheus.

It's essentially a better storage backend for Prometheus that in our case uses [Apache Cassandra](https://cassandra.apache.org/) for storage.

# Configuration

The absolute minimum to configure is:
```yml
cortex_storage_keyspace_name: 'cortex_metrics'
cortex_storage_consistency: 'QUORUM'
cortex_storage_replication_factor: 3
cortex_storage_retention_period: '50w' 
```

# Management

You can check the service status using `systemctl`:
```
 > sudo systemctl status cortex
admin@master-01.do-ams3.metrics.hq:~ % sudo systemctl status cortex
● cortex.service - "Cortex - Horizontally scalable, highly available, multi-tenant, long term Prometheus"
     Loaded: loaded (/lib/systemd/system/cortex.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2020-08-06 17:58:24 UTC; 13s ago
       Docs: https://cortexmetrics.io/
   Main PID: 79738 (cortex)
      Tasks: 9 (limit: 9451)
     Memory: 15.8M
     CGroup: /system.slice/cortex.service
             └─79738 /usr/local/bin/cortex -config.file=/etc/cortex/config.yml -schema-config-file /etc/cortex/schema.yml

Aug 06 17:58:24 master-01.do-ams3.metrics.hq cortex[79738]: level=info ts=2020-08-06T17:58:24.589568597Z caller=module_service.go:58 msg=initialising module=table-manager
Aug 06 17:58:24 master-01.do-ams3.metrics.hq cortex[79738]: level=info ts=2020-08-06T17:58:24.589638243Z caller=table_manager.go:333 msg="synching tables" expected_tables=2
Aug 06 17:58:24 master-01.do-ams3.metrics.hq cortex[79738]: level=info ts=2020-08-06T17:58:24.590262699Z caller=module_service.go:58 msg=initialising module=store
Aug 06 17:58:24 master-01.do-ams3.metrics.hq cortex[79738]: level=info ts=2020-08-06T17:58:24.59032967Z caller=module_service.go:58 msg=initialising module=ingester
Aug 06 17:58:24 master-01.do-ams3.metrics.hq cortex[79738]: level=info ts=2020-08-06T17:58:24.590386742Z caller=lifecycler.go:490 msg="not loading tokens from file, tokens file path is empty"
Aug 06 17:58:24 master-01.do-ams3.metrics.hq cortex[79738]: level=info ts=2020-08-06T17:58:24.590614721Z caller=module_service.go:58 msg=initialising module=querier
Aug 06 17:58:24 master-01.do-ams3.metrics.hq cortex[79738]: level=info ts=2020-08-06T17:58:24.591028648Z caller=cortex.go:319 msg="Cortex started"
```
And check the currently used config through the `/config` path:
```
admin@master-01.do-ams3.metrics.hq:~ % curl -s localhost:9092/config | grep keyspace   
    keyspace: cortex_metrics
```
