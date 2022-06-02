## average node latency during the last 5 minutes
rate(node_latency_sum[5m]) / rate(node_latency_count[5m])

## 0.95 quantile of node latency during the last 5 minutes
histogram_quantile(0.95, sum(
    rate(node_latency_bucket[5m])
) by (origin_node, destination_node, le))

## average app request duration during the last 5 minutes
sum(
    rate(istio_request_duration_milliseconds_sum{reporter="source", source_app!="unknown", destination_app!="unknown"}[5m])
) by (source_app, destination_app) / 
sum(
    rate(istio_request_duration_milliseconds_count{reporter="source", source_app!="unknown", destination_app!="unknown"}[5m])
) by (source_app, destination_app)

## 0.95 quantile of app request duration during the last 5 minutes
histogram_quantile(0.95, sum(
    rate(istio_request_duration_milliseconds_bucket{reporter="source", source_app!="unknown", destination_app!="unknown"}[5m])
) by (source_app, destination_app, le))

## average tcp bytes exchanged during the last 5 minutes
sum(
    rate(istio_tcp_sent_bytes_total{reporter="source", source_app!="unknown", destination_app!="unknown"}[5m]) 
    + 
    rate(istio_tcp_received_bytes_total{reporter="source", source_app!="unknown", destination_app!="unknown"}[5m])
) by (source_app, destination_app)

## average tcp connections opened during the last 5 minutes
sum(
    rate(istio_tcp_connections_opened_total{reporter="source", source_app!="unknown", destination_app!="unknown"}[5m]) 
) by (source_app, destination_app)

## average http/grpc traffic during the last 5 minutes
(sum(
    rate(istio_request_bytes_sum{reporter="source", source_app!="unknown", destination_app!="unknown"}[5m])
) by (source_app, destination_app)
+
sum(
    rate(istio_response_bytes_sum{reporter="source", source_app!="unknown", destination_app!="unknown"}[5m])
) by (source_app, destination_app)
)

## average request count during the last 5 minutes
sum(
    rate(istio_requests_total{reporter="source", source_app!="unknown", destination_app!="unknown"}[5m])
) by (source_app, destination_app)

## average traffic during the last 5 minutes
(sum(
    rate(istio_request_bytes_sum{reporter="source", app="sock-shop" source_workload!="unknown", destination_workload!="unknown"}[5m])
) by (source_workload, destination_workload)
+
sum(
    rate(istio_response_bytes_sum{reporter="source", app="sock-shop", source_workload!="unknown", destination_workload!="unknown"}[5m])
) by (source_workload, destination_workload)
) / 1000
or 
sum(
    rate(istio_tcp_sent_bytes_total{reporter="source", app="sock-shop", source_workload!="unknown", destination_workload!="unknown"}[5m]) 
    + 
    rate(istio_tcp_received_bytes_total{reporter="source", app="sock-shop", source_workload!="unknown", destination_workload!="unknown"}[5m])
) by (source_workload, destination_workload) / 1000