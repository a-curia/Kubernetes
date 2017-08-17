2 Kubernetes Services

# deploy an app and then just servers the hostname - this will create a deployment
	kubectl run demo --image=master.turbot:5000/serve-hostanme:last --replicas=2

# create a service for the app using svc.yaml file
	apiVersion: v1
	kind: Service
	metadata:
		name: demo
	spec:
		ports:
			- port: 80
			  targetPort: 8080
		selector:
			run: demo

# create service using yaml file
	kubectl create -f svc.yaml
	kubectl get service demo
	kubectl describe service demo