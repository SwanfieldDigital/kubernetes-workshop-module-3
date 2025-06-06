# Logging with Loki

**This installation is required only once per cluster.**

## Install Loki and configure Grafana

[loki](https://artifacthub.io/packages/helm/grafana/loki)
[promtail](https://artifacthub.io/packages/helm/grafana/promtail)

```shell
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm upgrade --install loki grafana/loki --namespace monitoring -f values-loki.yaml --version 4.5.1
helm upgrade --install promtail grafana/promtail --namespace monitoring -f values-promtail.yaml --version 6.8.2
kubectl apply -f datasource.yaml
```

## Restart Grafana

```shell
kubectl rollout restart deployment -n monitoring prom-grafana
```

## Loki features

### Explore Loki in Grafana

* In Grafana create an alerting rule from one of Loki's logs. For example use this query:

  ```sql
  sum by (pod) (count_over_time({namespace="kube-system"} |~ "WARNING" [5m]))
  ```

* Example

![Loki Alerting Rule](img/loki-grafana-alert-rule.png)
