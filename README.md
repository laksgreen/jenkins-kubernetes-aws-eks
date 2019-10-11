Setup Jenkins Master in Kuberenetes (AWS-EKS):
---------------------------------------------
Create a new EKS cluster set up:
-------------------------------
$ eksctl create cluster -f eks.yaml 

Install IAM authenticator:

Update the kubeconfig file in your current working user account.

$ aws eks --region us-west-2 update-kubeconfig --name eks-cluster-name

Views:
----- 
Get nodes, config, namespaces, services:

$ kubectl get nodes

$ kubectl config get-contexts

$ kubectl get namespaces

$ kubectl get svc

$ sudo kubectl config set-context eks-cluster-name (EKS cluster name)


WORKSTATION:
------------

Create a namespace,

$ kubectl create -f jenkins-namespace.yaml

Jump into your namespace which is created,

$ kubectl config set-context --current --namespace=jenkins-lts

Validate it,

$ kubectl config view | grep namespace:

Create Persistence volume (PV)

$ kubectl get storageclass  # refer the AWS documents(https://docs.aws.amazon.com/eks/latest/userguide/storage-classes.html)
$ kubectl create -f  jenkins-kubernetes-master-per-vol.yaml
$ kubectl get pv

Create Persistence volume cliam(PVC):

$ kubectl create -f jenkins-kubernetes-master-per-volclaim.yaml
$ kubectl get pvc
$ kubectl describe svc jenkins-master-pvc

Create Service (SVC)

$ kubectl create -f jenkins-kubernetes-master-svc.yaml
$ kubectl get svc
$ kubectl describe svc jenkins-master-svc

Create Pods(POD):

$ kubectl create -f jenkins-kubernetes-master-deploy.yaml
$ kubectl get pods
$ kubectl describe pods jenkins-leader-5486759d97-mcwsk  # after the pod name the dynamic name will be added

Dashboard(Mintenance):

View the kubernetes dashboard on your browser:

$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0-beta4/aio/deploy/recommended.yaml
$ kubectl -n kubernetes-dashboard get secret
$ kubectl -n kubernetes-dashboard describe secrets kubernetes-dashboard-token-j7h4j
On browser: http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#!/login
