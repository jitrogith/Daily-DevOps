# Setup Kubernetes (K8s) Cluster on AWS
####  1. Create Ubuntu EC2 instance
####  2. Install AWSCLI
       curl https://s3.amazonaws.com/aws-cli/awscli-bundle.zip -o awscli-bundle.zip
       apt install unzip
       unzip awscli-bundle.zip
       #sudo apt-get install unzip - if you dont have unzip in your system
       ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
####  3. Install kubectl on ubuntu instance
       curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
       chmod +x ./kubectl
       sudo mv ./kubectl /usr/local/bin/kubectl
####  4. Install kops on ubuntu instance
       curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
       chmod +x kops-linux-amd64
       sudo mv kops-linux-amd64 /usr/local/bin/kops
####  5. Create an IAM user/role with Route53, EC2, IAM and S3 full access
####  6. Attach IAM role to ubuntu instance
       # Note: If you create IAM user with programmatic access then provide Access keys. Otherwise region information is enough
       aws configure
####  7. Create a Route53 private hosted zone (you can create Public hosted zone if you have a domain)
       Routeh53 --> hosted zones --> created hosted zone  
       Domain Name: jitrobittikaka.com
       Type: Private hosted zone for Amzon VPC
####  8. Create an S3 bucket
       aws s3 mb s3://demo.k8s.jitrobittikaka.com
####  9. Expose environment variable:
       export KOPS_STATE_STORE=s3://demo.k8s.jitrobittikaka.com
#### 10. Create sshkeys before creating cluster
       ssh-keygen
#### 11. Create kubernetes cluster definitions on S3 bucket
       kops create cluster --cloud=aws --zones=ap-south-1b --name=demo.k8s.jitrobittikaka.com --dns-zone=jitrobittikaka.com --dns private 
#### 12. Create kubernetes cluser
       kops update cluster demo.k8s.jitrobittikaka.com --yes
#### 13. Validate your cluster
       kops validate cluster
#### 14. To list nodes
       kubectl get nodes
#### 15. To delete cluster
       kops delete cluster demo.k8s.jitrobittikaka.com --yes

# Deploying Nginx pods on Kubernetes
#### Deploying Nginx Container
      kubectl run sample-nginx --image=nginx --replicas=2 --port=80
      # kubectl run simple-devops-project --image=yankils/simple-devops-image --replicas=2 --port=8080
      kubectl get pods
      kubectl get deployments
#### Expose the deployment as service. This will create an ELB in front of those 2 containers and allow us to publicly access them.
      kubectl expose deployment sample-nginx --port=80 --type=LoadBalancer
      # kubectl expose deployment simple-devops-project --port=8080 --type=LoadBalancer
      kubectl get services -o wide
