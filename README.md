LOAD BALANCER

Quickstart
git clone https://github.com/Samk1710/load-balancer
cd load-balancer/nginx-k8s

minikube start --driver=docker
eval $(minikube docker-env)

docker build -t nginx-pod-a -f Dockerfile-pod-a .
docker build -t nginx-pod-b -f Dockerfile-pod-b .
docker build -t nginx-pod-c -f Dockerfile-pod-c .

kubectl apply -f deployments.yaml
kubectl apply -f loadbalancer.yaml

Verify Deployment
bash
Copy
kubectl get all
Access the Service
bash
Copy
minikube service webapp-loadbalancer --url
# Test with: curl http://<service-ip>:<port>

Demo Load Balancing
bash
Copy
for i in {1..6}; do
  curl -s $(minikube service webapp-loadbalancer --url)
done
View Pod Contents
bash
Copy
kubectl exec $(kubectl get pods -l version=a -o name) -- cat /usr/share/nginx/html/index.html
kubectl exec $(kubectl get pods -l version=b -o name) -- cat /usr/share/nginx/html/index.html
kubectl exec $(kubectl get pods -l version=c -o name) -- cat /usr/share/nginx/html/index.html
