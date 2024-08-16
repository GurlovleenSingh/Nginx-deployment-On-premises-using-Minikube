# Nginx-deployment-On-premises-using-Minikube
Deploying Nginx on a Kubernetes (K8s) cluster in a local or on-premises setup
Deploying Nginx on a Kubernetes (K8s) cluster without relying on a cloud provider involves setting up Kubernetes on a local or on-premises environment. You can use tools like Minikube, KinD (Kubernetes in Docker), or a more traditional installation on physical or virtual machines.

Here’s a step-by-step guide for deploying Nginx on a Kubernetes cluster in a local or on-premises setup:

Prerequisites
1.Kubernetes Cluster: A running Kubernetes cluster. You can use Minikube, KinD, or a manually configured cluster.
2.kubectl: The Kubernetes command-line tool installed and configured to communicate with your cluster.

Step-by-Step Guide
1. Set Up Your Kubernetes Cluster
Using Minikube:
Install Minikube:
For Linux:
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube-linux-amd64
sudo mv minikube-linux-amd64 /usr/local/bin/minikube
2. start minikube
minikube start

2. Deploy Nginx on Kubernetes

Create a Deployment YAML File:
Create a file named nginx-deployment.yaml with the following content:
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

kubectl apply -f nginx-deployment.yaml

3. Expose Nginx with a Service:
Create a file named nginx-service.yaml with the following content:
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30001

Explanation:

type: NodePort: Exposes the service on each node’s IP at a static port (nodePort), which is accessible from outside the cluster.
nodePort: 30001: This specifies the port on which the service will be accessible. You can choose a port in the range 30000-32767.

Apply the Service Configuration:
kubectl apply -f nginx-service.yaml
4. Verify the Deployment and Service:
Check the status of the deployment and service:
kubectl get deployments
kubectl get services
Output Example:

NAME                READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment    2/2     2            2           1m

NAME            TYPE       CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
nginx-service   NodePort   10.96.0.1    <none>        80:30001/TCP   1m

5. Access Nginx:
Minikube: Use Minikube’s built-in service command to get the URL:
minikube service nginx-service

6.Access Nginx using the IP address of your node and the NodePort you specified. 
For example, if your node IP is 192.168.99.100
http://192.168.99.100:30001

Summary:
Set up a Kubernetes cluster using Minikube, KinD, or manual installation.
Create a Deployment YAML file for Nginx and apply it.
Create a Service YAML file to expose Nginx and apply it.
Verify the deployment and service and access Nginx via the exposed port.
This setup should get Nginx up and running on your local or on-premises Kubernetes cluster. If you encounter any issues, check the logs of your pods and services using kubectl logs and kubectl describe commands for more details.
















