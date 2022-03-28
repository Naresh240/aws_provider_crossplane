# cross-plane

# Pre-Requisites:
  - Minikube Cluster
  - Create User (For communicating aws services with crossplane)
# Minikube Cluster Setup
  [minikube](https://github.com/Naresh240/kubernetes/tree/main/minikube-setup)
# Create User in AWS
  Create user with admin policy and get AWS_ACCESS_KEY and AWS_SECRET_ACCESS_KEY
# export keys and save in file as below
`````
  export aws_access_key_id=XXXXXXXXXXXXXXXXXXXXXXXXXXX
  export aws_secret_access_key=XXXXXXXXXXXXXXXXXXXXXXXXXXXX
  
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
check all using below commands
  kubectl get vpc
  kubectl get subnets
  kubectl get internetgateway
  kubectl get routetable
`````  
# vpc-xrd-composities
  kubectl  apply -f xrd.yml
  kubectl apply -f composition.yml
  kubectl apply -f claim.yml
``````
