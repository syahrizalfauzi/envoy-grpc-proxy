# Envoy gRPC Proxy
This envoy config & docker compose YAML will run an envoy proxy that acts as a proxy from browser with gRPC-web (HTTP1) to a backend gRPC service (HTTP2)

The config assumes you have the gRPC service running locally (`host.docker.internal`) at port `10000`, you can change it in `envoy.yaml`
```yaml
  clusters:
    - name: grpc_service
      connect_timeout: 0.25s
      type: LOGICAL_DNS
      http2_protocol_options: {}
      lb_policy: ROUND_ROBIN
      load_assignment:
        cluster_name: grpc_service
        endpoints:
          - lb_endpoints:
            - endpoint:
                address:
                  socket_address:
                    address: host.docker.internal # assuming the gRPC service is running on the host machine
                    port_value: 10000
```
With the default port mapping, the web app can hit to port `9090` and you can access envoy admin page at port `9901`
```yaml
    ports:
      - "9901:9901"
      - "9090:9090"
```

To start, run docker compose (`-d` for detached mode)
```sh
docker compose up --build -d
```
