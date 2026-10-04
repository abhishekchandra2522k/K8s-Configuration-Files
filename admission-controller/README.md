# Commands to create and run the admission controller pre-requisites

1 kubectl create ns secure-ops
2  kubectl label namespace secure-ops pod-security.kubernetes.io/enforce=restricted
3  kubectl get ns --show-labels
4  kubectl create sa secure-sa -n secure-ops 
5  kubectl api-resources
6  kubectl create role pod-reader --verb=get,list,watch --resource=pods -n secure-ops
7  kubectl create rolebinding secure-binding --role=pod-reader --user=secure-sa -n secure-ops 
8  kubectl get sa,role -n secure-ops 
9  kubectl describe role pod-reader -n secure-ops 
10  kubectl describe rolebinding secure-binding -n secure-ops 
