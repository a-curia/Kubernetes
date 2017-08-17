## Kubernetes Pod Draining for Node Maintenance
# usefull if you need to do node Maintenance

# deploy an app
	kubectl run demo --image=master.tubot:5000/fedora:24-demo --replicas=2

# pod are spread on different nodes
	kubectl get pods -o yaml | grep nodeName:

# drain node1
	kubectl drain node1.turbot

# pods are all on node2
	kubectl get pods -o yaml | grep nodeName:

# uncordon node1
	kubectl uncordon node1.turbot

# if you want to force a rebalance - scale down, then back up (rescheduler in development)
	kubectl scale deployments/demo --replicas=0
	kubectl scale deployments/demo --replicas=2

# now pods will be spread on different nodes
	kubectl get pods -o yaml | grep nodeName:

	kubectl delete deployment demo