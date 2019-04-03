Install aws, kubectl and kops

   1  install pip on RHEL Box.

        curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
        sudo python get-pip.py 
   
   2 Install aws cli  
        
        pip install awscli 
   
   3  create IAM user with below Permission.
   
      AmazonEC2FullAccess
      IAMFullAccess
      AmazonS3FullAccess
      AmazonVPCFullAccess
      AmazonRoute53FullAccess

   4  configure user to access aws through command line interface. (access key and secret key)
   5  aws configure
   6  Install kubectl

 
         curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
         chmod +x ./kubectl
         sudo mv ./kubectl /usr/local/bin/kubectl

   7  Install Kops 
         wget -O kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d ‘“’ -f 4)/kops-linux-amd64
         chmod +x ./kops
         sudo mv ./kops /usr/local/bin/kops

  8  export AWS_ACCESS_KEY_ID=XXXXXXXX
  9  export AWS_SECRET_ACCESS_KEY=XXXXXXXXX
  10  ssh-keygen -t rsa
  11  export CLUSTER_NAME=myfirstcluster.k8s.local
  12  export KOPS_STATE_STORE=s3://kubernetescluster-kube
  13  kops create cluster --zones us-east-1a --name=${NAME}

  14  kops update cluster myfirstcluste.k8s.local --yes
  15  kubectl get nodes



  16  You can then edit the cluster settings with one of these commands:


        List clusters with: kops get cluster
        Edit this cluster with: kops edit cluster $CLUSTER_NAME
        Edit your node instance group: kops edit ig --name=$CLUSTER_NAME nodes
        Edit your master instance group: kops edit ig --name=CLUSTER_NAME master-us-east-1a

  17  To enable the Kubernetes UI you need to install the UI service:
  
       kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml
 
  18  Then you can use the kubctl proxy to access the UI from your machine:
 
       kubectl proxy --port=8080 &
       The UI should now be available at http://localhost:8080



  19  Deploying Nginx Container To test our new Kubernetes cluster, we could deploy a simple service made up of some nginx containers:

       Create an nginx deployment:
       kubectl run sample-nginx --image=nginx --replicas=2 --port=80
       kubectl get pods
       kubectl get deployments

  20  Expose the deployment as service. This will create an ELB in front of those 2 containers and allow us to publicly access them:

       kubectl expose deployment sample-nginx --port=80 --type=LoadBalancer
      
       kubectl get services -o wide

  21  delete cluster $CLUSTER_NAME --yes 
