# Important Commands

- `kubectl run curl-test --image=curlimages/curl --restart=Never --rm -it --   curl http://sample-app.default.svc.cluster.local:8080/metrics` 

The above command can be used to run a pod and run a curl test to check the svc endpoint is functional or not. Check the deployment-service.yaml in the same dir for reference.


## Grafana Commands (Installed via Helm Chart)

List Configmaps
- `kubectl get configmaps -n monitoring | grep -i datasource`
Read configmap yaml
- `kubectl get configmap kps-kube-prometheus-stack-grafana-datasource -n monitoring -o jsonpath='{.data.datasource\.yaml}'`
Validate the configmap yaml from the grafana pod container
- `kubectl exec -n monitoring $(kubectl get pod -n monitoring -l app.kubernetes.io/name=grafana -o name | head -1) -c grafana -- cat /etc/grafana/provisioning/datasources/datasource.yaml`
List sidecar containers
- `kubectl get pod -n monitoring -l app.kubernetes.io/name=grafana -o jsonpath='{.items[0].spec.containers[*].name}'`

