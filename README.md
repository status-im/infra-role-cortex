# Description

This role configures [Cortex](https://cortexmetrics.io/) - a Horizontally scalable, highly available, multi-tenant, long term Prometheus.

It's essentially a better storage backend for Prometheus that in our case uses [Digital Ocean Spaces](https://www.digitalocean.com/products/spaces) for storage.

# Configuration

The absolute minimum to configure is:
```yml
cortex_blocks_storage_endpoint: 'ams3.digitaloceanspaces.com'
cortex_blocks_storage_bucket: 'my-metrics-storage-bucket'
cortex_blocks_storage_key_id: 'super-secret-id'
cortex_blocks_storage_secret: 'super-secret-key'
```

# Management

You can check the service status using `systemctl`:
```
 > sudo systemctl -o cat status cortex
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

level=info ts=2020-08-06T17:58:24.589568597Z caller=module_service.go:58 msg=initialising module=table-manager
level=info ts=2020-08-06T17:58:24.589638243Z caller=table_manager.go:333 msg="synching tables" expected_tables=2
level=info ts=2020-08-06T17:58:24.590262699Z caller=module_service.go:58 msg=initialising module=store
level=info ts=2020-08-06T17:58:24.59032967Z caller=module_service.go:58 msg=initialising module=ingester
level=info ts=2020-08-06T17:58:24.590386742Z caller=lifecycler.go:490 msg="not loading tokens from file, tokens file path is empty"
level=info ts=2020-08-06T17:58:24.590614721Z caller=module_service.go:58 msg=initialising module=querier
level=info ts=2020-08-06T17:58:24.591028648Z caller=cortex.go:319 msg="Cortex started"
```
And check the currently used config through the `/config` path:
```
admin@master-01.do-ams3.metrics.hq:~ % curl -s localhost:9092/config | grep keyspace
    keyspace: cortex_metrics
```

# Web UI

Cortex also exposes a very basic UI on the root of it's main port(`9092` by default).

The most interesting paths would include `/config`, `/services`, and `/ingester/ring`.
