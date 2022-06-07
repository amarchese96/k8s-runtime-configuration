- Install mentat component
```
kubectl apply -f mentat.yml
```

- Install Istio
```
istioctl install --set profile=demo -y
```

- Label default namespace
```
kubectl label namespace default istio-injection=enabled
```

- Install prometheus server
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/prometheus -n istio-system --values observability/prometheus-values.yml
```

- Install grafana dashboard
```
helm repo add grafana https://grafana.github.io/helm-charts
helm install grafana grafana/grafana -n istio-system --values observability/grafana-values.yml
```

- Install jaeger
```
kubectl apply -f observability/jaeger.yml
```

- Install kiali
```
kubectl apply -f observability/kiali.yml
```

- Clean up
```
kubectl delete -f observability/kiali.yml
kubectl delete -f observability/jaeger.yml
helm uninstall grafana -n istio-system
helm uninstall prometheus -n istio-system
istioctl manifest generate --set profile=demo | kubectl delete --ignore-not-found=true -f -
kubectl delete namespace istio-system
kubectl label namespace default istio-injection-
```