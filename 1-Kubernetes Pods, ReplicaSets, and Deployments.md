# Kubernetes Pods, ReplicaSets, and Deployments

# create a pod.yaml file

  apiVersion: v1
  kind: Pod
  metadata:
    name: demo
  spec:
    containers:
    - image: master.turbot:5000/fedora:24-demo
      name: fedora
      imagePullPolicy: Always
  
# run command to create pod
  kubectl create -f pod.yaml

# check to see if pod is running
  kubectl get pod demo
  kubectl logs --tail=3 demo

# delete the pod
  kubectl delete pod demo


# create a ReplicaSet using replicaset.yaml file

  apiVersion: extensions/v1beta1
  kind: ReplicaSet
  metadata:
    name: demo
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: demo
    template:
      metadata:
        labels:
          app: demo
      spec:
        containers:
        - name: fedora
          image: master.turbot:5000/fedora:24-demo
          imagePullPolicy: Always
  
# run command to create the replica replicaset
  kubectl create -f replicaset.yaml
  kubectl get pods

  kubectl scale --replicas=4 replicaset/demo
  kubectl delete pod demo-deq212

  kubectl delete replicaset demo
# limitation is that you cannot change the pod template used in replicaset
# there is another concept called deployment that have the notion of Appplication Lifecicle

# create file deployment.yaml
  apiVersion: extensions/v1beta1
  kind: Reployment
  metadata:
    name: demo
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: demo
    template:
      metadata:
        labels:
          app: demo
      spec:
        containers:
        - name: fedora
          image: master.turbot:5000/fedora:24-demo
          imagePullPolicy: Always
          env:
          - name: VERSION
            value: "v1"
    
# Deployments create ReplicaSets which creates Pods

  kubectl create -f deployment.yaml

  kubectl get deployment demo
  kubectl get replicasets
  kubectl get pods
  
  kubectl logs --tail=3 demo-2433213-333
  kubectl logs --tail=3 demo-2343556-fdw

# lets change the pod environment variable to v2
# can also be done with 'kubectl edit' or 'kubectl apply -f'
  kubectl patch deployment demo -p '{"spec":{"template":{"spec":{"containers":[{"name":"fedora", "env":[{"name":"VERSION", "value":"v2"}]}]}}}}'

  kubectl get replicasets

  kubectl logs --tail=3 demo-dfw231323-433

  kubectl rollout undo deployment/demo

  kubectl delete deployment demo