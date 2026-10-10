# Few example of PromQL queries 

1. To fetch the pods in ready condition aggregated with namespace - `sum by (namespace) (kube_pod_status_ready{condition="true"} == 1)`
2. To fetch the pods per namespace `count by (namespace) (kube_pod_info)`
