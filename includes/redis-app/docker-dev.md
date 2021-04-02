```bash
version: "3.4"

services:
  redis:
    container_name: redismod
    image: redislabs/redismod:latest
    ports:
      - 6379:6379/tcp
    # Uncomment and edit the local path in the following line to have
    # Redis' data persisted to the host's filesystem.
    # volumes:
    #   - ./dump.rdb:/data/dump.rdb

  grafana:
    container_name: grafana
    image: grafana/grafana:latest
    ports:
      - 3000:3000/tcp
    environment:
      - GF_AUTH_ANONYMOUS_ORG_ROLE=Admin
      - GF_AUTH_ANONYMOUS_ENABLED=true
      - GF_AUTH_BASIC_ENABLED=false
      - GF_ENABLE_GZIP=true
      - GF_USERS_DEFAULT_THEME=light
      - GF_PLUGINS_ALLOW_LOADING_UNSIGNED_PLUGINS=redis-app,redis-datasource
      # Uncomment to run in debug mode
      # - GF_LOG_LEVEL=debug
    volumes:
      # Redis Data Source should be cloned and built
      - ../../grafana-redis-datasource/dist:/var/lib/grafana/plugins/redis-datasource
      - ../dist:/var/lib/grafana/plugins/redis-app
      - ../provisioning/datasources:/etc/grafana/provisioning/datasources
      - ../provisioning/plugins:/etc/grafana/provisioning/plugins
      # Uncomment to preserve Grafana configuration
      # - ./data:/var/lib/grafana
```