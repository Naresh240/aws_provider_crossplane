# cross-plane

# Pre-Requisites:
  - Minikube Cluster
  - Create User (For communicating aws services with crossplane)
# Minikube Cluster Setup
  [minikube](https://github.com/Naresh240/kubernetes/tree/main/minikube-setup)
# Install Helm
````
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
`````
# Install Cross-plane
`````
kubectl create namespace crossplane-system

helm repo add crossplane-stable https://charts.crossplane.io/stable
helm repo update
helm install crossplane --namespace crossplane-system crossplane-stable/crossplane
`````
# Create User in AWS
  Create user with admin policy and get AWS_ACCESS_KEY and AWS_SECRET_ACCESS_KEY
# export keys and save in file as below
`````
export AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXXXXXXXXXXXXX
export AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXX

echo "[default]
aws_access_key_id = $AWS_ACCESS_KEY_ID
aws_secret_access_key = $AWS_SECRET_ACCESS_KEY
" >aws-creds.conf
`````
# Create a Kubernetes secret with the configuration file generated
`````
kubectl create secret generic aws-creds -n crossplane-system --from-file=creds=./aws-creds.conf
`````
# Create the Provider config for our AWS account
`````  
kubectl apply -f aws-provider.yml
kubectl apply -f providerConfig.yml
````` 
# vpc-direct
`````
kubectl apply -f vpc-direct/

kubectl get vpc
kubectl get subnets
kubectl get internetgateway
kubectl get routetable
`````  
# vpc-xrd-composities
`````
kubectl  apply -f xrd.yml
kubectl apply -f composition.yml
kubectl apply -f claim.yml
``````
