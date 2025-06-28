# Appscrip-Assignment

This project demonstrates the end-to-end provisioning of an Amazon EKS cluster using Terraform and deployment of a containerized NGINX application via Kubernetes manifests. It integrates GitOps using ArgoCD for continuous deployment and includes optional Ingress setup with Route 53 and a custom domain. The entire workflow is automated and infrastructure is managed as code for reproducibility and scalability.

## Install all Services required
Change the working directory to terraform/Scripts
```
#make permissionex.sh file to executable
chmod +x permissionex.sh
```
Now Install each Service one by one,

### Install AWS CLI
```
./aws-cli.sh
```
Verify Installation
```
aws --version
```
Configure AWS Console
```
aws configure
#Enter details of your AWS instance
```
### Install Terraform
```
./terraform.sh
```
Verify Installation
```
terraform -v
```
### Install Kubernetes & EKS 
```
./kubectl.sh
./eksctl.sh
```
Now, change directory back to terraform/
```
cd ..
```
### Execute Terraform Configurations:
Check or Step into project directory,

Initialize Terraform modules
```
terraform init
```
Review the execution plan
```
terraform plan
```
Apply the infrastructure changes
```
terraform apply --auto-approve
```
### Access your EKS cluster:
Update kubeconfig
```
aws eks --region ap-south-1 update-kubeconfig --name eks-cluster
```
Verify cluster access
```
kubectl get nodes
```
## ArgoCD Login Instructions

### 1. Install ArgoCD:
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
### 2. Expose ArgoCD via LoadBalancer:
```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
kubectl get svc argocd-server -n argocd
```
### 3. Retrieve the Admin Password:
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
### 4. Access ArgoCD:
```
* Search this URL: http://<external-ip>:80
* Username: admin
* Password: Retrieved above
```
### 5. Create & Apply ArgoCD Application:
```
kubectl apply -f ArgoCD_app.yaml
```

## Public Access for NGINX 
### 1. LoadBalancer (Public Access)
Change the Server type:
```
kubectl patch svc nginx-service -p '{"spec": {"type": "LoadBalancer"}}'
```
Retrieve the IP,
```
kubectl get svc nginx-service
```
Visit the URL:
```
http://<external-ip>
```
## ** Conclusion **
This README covers all the commands used to provisions an Amazon EKS cluster using Terraform, deploys an NGINX web application via Kubernetes manifests, manages deployments using Argo CD (GitOps).

### 📧 Maintained By
#### KARTIK KAIN
#### Analyst, HCLTech | Cloud & DevOps Engineer
[LinkedIN](https://www.linkein.com/in/kartikkain) | [GitHub](https://github.com/MrKainn)




