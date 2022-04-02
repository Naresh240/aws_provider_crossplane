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
# Export keys and save in file as below
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
# RDS
`````
kubectl apply -f rds/

kubectl get vpc
kubectl get subnets
kubectl get internetgateway
kubectl get routetable
kubectl get securitygroup
kubectl get rdsinstance
`````  
# Decode Database Password
  Secret will create "production-rds-conn-string" with this name. we need to debug in different approches
 
```` Approch: 1 ````
````
kubectl get secrets/production-rds-conn-string --template='{{.data.password | base64decode}}'
````
```` Approch: 2 ````
`````
kubectl edit secrets production-rds-conn-string

# Copy password and decode it using below step
echo -n "encode-password" | base64 --decode
`````
# Connect to database
`````
mysql -u <username> -p <decoded-password>

# default values:
  username=admin
  database=mysql
  password=<decoded-password>
  host=<end-point>
`````
