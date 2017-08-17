** Kubernetes Deploying Wordpress

# Create a persistent volumes for wordpress and mysql data

kubectl create -f wordpress-pvs.yaml

# Create a secret for mysql root password
kubectl create secret generic mysql-pass --from-literal='password=younewerguess'

# Create mysql persistent volume claim
cat pvc.yaml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
	name: mysql-pv-claim
	labels:
		app: wordpress
spec:
	accessModes:
		- ReadWriteOnce
	resources:
		requests:
			storage: 2Gi
			
kubectl create -f pvc.yaml


