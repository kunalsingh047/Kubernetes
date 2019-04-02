Install aws, kubectl and kops

   1  apt-get update;apt-get install python-pip
   2  pip install awscli --upgrade –user
        Or
       sudo apt  install awscli
   3  export PATH=~/.local//bin:$PATH
   4  source ~/.profile
   5  aws
   6  aws configure
   7  curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
   8  chmod +x ./kubectl
   9  sudo mv ./kubectl /usr/local/bin/kubectl
  10  wget -O kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d ‘“’ -f 4)/kops-linux-amd64
  11  chmod +x ./kops
  12  sudo mv ./kops /usr/local/bin/
  13  export AWS_ACCESS_KEY_ID=XXXXXXXX
  14  export AWS_SECRET_ACCESS_KEY=XXXXXXXXX
  15  ssh-keygen -t rsa
  16  export NAME=myfirstcluster102.k8s.local
  17  export KOPS_STATE_STORE=s3://ankur-kube
  18  kops create cluster --zones us-east-1a ${NAME}

  19  kops update cluster myfirstcluster102.k8s.local --yes
  20  kubectl get nodes

ankurs [2:46 PM]
wget -O kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d ‘“’ -f 4)/kops-linux-amd64
wget -O kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d ‘“’ -f 4)/kops-linux-amd64
wget -O kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d ‘“’ -f 4)/kops-linux-amd64
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
