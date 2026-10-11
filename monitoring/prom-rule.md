# Few handy commands for prom rule (alerts) testing


Run this one-off traffic generator:

kubectl run load-gen \
  --image=curlimages/curl \
  --restart=Never \
  --rm -i --quiet -- \
  sh -c 'for i in $(seq 1 50); do curl -s http://sample-app.default.svc.cluster.local:9898/ > /dev/null; done; echo "Done"'

Then confirm the metric exists in Prometheus:

curl -s "http://localhost:9090/api/v1/query?query=http_requests_total" \


---

Trigger the high request rate alert

Run the traffic generator in the background using tmux so you can run other commands while traffic is still flowing:
  tmux new-session -d -s load-burst \
  "kubectl run load-burst --image=curlimages/curl --restart=Never --rm -i --quiet -- \
  sh -c 'for i in \$(seq 1 300); do curl -s http://sample-app.default.svc.cluster.local:9898/ > /dev/null; sleep 0.2; done'"

Wait about 45 seconds, then watch the Prometheus UI → Alerts tab. SampleAppHighRequestRate should move from inactive → pending → firing.

Once you see it firing, capture the rate immediately while traffic is still running:

curl -s -G \
  --data-urlencode 'query=rate(http_requests_total[1m])' \
  "http://localhost:9090/api/v1/query" \
  | jq '[.data.result[] | {instance: .metric.instance, rate: .value[1]}]' \
  > /home/user/output2.txt

cat /home/user/output2.txt
Then clean up:

tmux kill-session -t load-burst
kubectl delete pod load-burst --ignore-not-found=true


---

Trigger the app down alert

Scale sample-app down to zero replicas:

kubectl scale deployment sample-app --replicas=0 -n default
Watch the Prometheus UI → Alerts tab. The SampleAppDown alert should transition:

inactive → pending (Prometheus detects up == 0 but waits out the for: 1m window)

pending → firing (condition has persisted beyond 1 minute)

This takes approximately 90 seconds from the scale-down command.

Task 6 — Restore the app and confirm alerts resolve

Scale the deployment back up:

kubectl scale deployment sample-app --replicas=1 -n default
Wait about 60 seconds, then confirm both alerts return to inactive/0 active in the Prometheus UI.
