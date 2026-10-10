# Important Command

1. kubectl run curl-test --image=curlimages/curl --restart=Never --rm -it --   curl http://sample-app.default.svc.cluster.local:8080/metrics 

The above command can be used to run a pod and run a curl test to check the svc endpoint is functional or not. Check the deployment-service.yaml in the same dir for reference.
