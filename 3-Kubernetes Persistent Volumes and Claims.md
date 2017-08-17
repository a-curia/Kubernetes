## Kubernetes Persistent Volumes and Claims 

# create a demo persistent Volume - pv.yaml
	apiVersion: v1
	kind: PersistentVolume
	metadata:
		name: demo
	spec:
		capacity:
			storage: 5Gi
		accessModes:
			- ReadWriteOnce
		persistentVolumeReclaimPolicy: Recycle
		nfs:
			path: /srv/demo
			server: master.tubot
	
# to create the volume
	kubectl create -f pv.yaml

	kubectl get pv demo

# using the created volum - claiming the valume = pvc.yaml
	kind: PersistentVolumeClaim
	apiVersion: v1metadata:
		name: demo
	spec:
		accessModes:
			- ReadWriteOnce
		resources:
			requests:
				storage: 2Gi

# call the claim
	kubectl create -f pvc.yaml
	kubectl get pvc demo

# create a pod that mounts the volume - pvpod.yaml
	apiVersion: v1
	kind: Pod
	metadata:
		name: demo
	spec:
		containers:
			- image: master.turbot:5000/fedora:24-demo
			  name: fedora
			  imagePullPolicy: Always
			  volumeMounts:
			  - mounthPath: "/mnt"
			    name: demo
		volumes:
			  - name: demo
			    persistentVolumeClaim:
					claimName: demo
	
# create it
	kubectl create -f pvpod.yaml


# write data to the (alleged) persistent volume
	kubectl exec -ti demo -- /bin/sh -c "echo 'i hope this is persistent' > /mnt/mydata"

    kubectl exec -ti demo --cat /mnt/mydata

# delete the pod
	kubectl delete pod demo

# recreate the pod
	kubectl create -f pvpod.yaml

# check the persistent data is still there
	kubectl exec -ti demo --cat /mnt/mydata

# delte the pod
	kubectl delete pod demo

# delete the persistent volume claim
	kubectl delete pvc demo

# delete the persistent volume
	kubectl delete pv demo