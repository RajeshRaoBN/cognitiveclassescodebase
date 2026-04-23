cd home/project

[ ! -d 'CC201' ] && git clone https://github.com/ibm-developer-skills-network/CC201.git

cd CC201/labs/3_K8sScaleAndUpdate/

ls

export MY_NAMESPACE=sn-labs-$USERNAME

docker build -t us.icr.io/$MY_NAMESPACE/hello-world:1 . && docker push us.icr.io/$MY_NAMESPACE/hello-world:1

kubectl apply -f deployment.yaml

kubectl get pods

kubectl expose deployment/hello-world

kubectl proxy

curl -L localhost:8001/api/v1/namespaces/sn-labs-$USERNAME/services/hello-world/proxy

kubectl scale deployment hello-world --replicas=3

kubectl get pods

for i in `seq 10`; do curl -L localhost:8001/api/v1/namespaces/sn-labs-$USERNAME/services/hello-world/proxy; done

kubectl scale deployment hello-world --replicas=1

kubectl get pods

docker build -t us.icr.io/$MY_NAMESPACE/hello-world:2 . && docker push us.icr.io/$MY_NAMESPACE/hello-world:2

ibmcloud cr images

kubectl set image deployment/hello-world hello-world=us.icr.io/$MY_NAMESPACE/hello-world:2

kubectl rollout status deployment/hello-world

kubectl get deployments -o wide

curl -L localhost:8001/api/v1/namespaces/sn-labs-$USERNAME/services/hello-world/proxy

kubectl rollout undo deployment/hello-world

kubectl rollout status deployment/hello-world

kubectl get deployments -o wide

kubectl create configmap app-config --from-literal=MESSAGE="This message came from a ConfigMap!"

docker build -t us.icr.io/$MY_NAMESPACE/hello-world:3 . && docker push us.icr.io/$MY_NAMESPACE/hello-world:3

kubectl apply -f deployment-configmap-env-var.yaml

kubectl delete configmap app-config && kubectl create configmap app-config --from-literal=MESSAGE="This message is different, and you didn't have to rebuild the image!"

kubectl rollout restart deployment hello-world

curl -L localhost:8001/api/v1/namespaces/sn-labs-$USERNAME/services/hello-world/proxy

kubectl delete -f deployment-configmap-env-var.yaml

kubectl delete service hello-world
