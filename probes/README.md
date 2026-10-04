# Liveness Probe History cmds

    1  vim liveness-probe.yaml
    2  kubectl create ns resiliency-lab
    6  kubectl apply -f liveness-probe.yaml 
    7  vim liveness-probe.yaml
    8  kubectl apply -f liveness-probe.yaml 
    9  vim liveness-probe.yaml
   14  kubectl apply -f liveness-probe.yaml 
   18  watch -n 1 kubectl get po -n resiliency-lab 
   20  kubectl apply -f liveness-probe.yaml 
   21  watch -n 1 kubectl get po -n resiliency-lab 
   22  kubectl autoscale deployment -n resiliency-lab payment-service --name=payment-hpa --cpu-percent=50 --min=1 --max=5
   23  watch -n 1 kubectl get po -n resiliency-lab 
