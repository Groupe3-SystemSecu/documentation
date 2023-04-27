# Création de cluster EKS

## Création d'un utilisateur IAM et création des politiques de sécurité nécessaires + politiques déjà définies (documentation eksctl)
https://eksctl.io/usage/minimum-iam-policies/

## Installation de eksctl sur wsl
`curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp`
`sudo mv /tmp/eksctl /usr/local/bin`

## Gestion des credentials
Aller sur la page de l'utilisateur IAM dans la partie "Informations d'identification de sécurité" et "Créer une clé d'accès" (id de la clé + clé ) 

`mkdir ~/.aws`
`vim ~/.aws/credentials`
```ini
[default]
aws_access_key_id =
aws_secret_access_key=
```

## Installation de Kubectl nécessaire pour gérer le cluster en ligne de commande
`curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl`
`chmod +x ./kubectl`
`sudo mv ./kubectl /usr/local/bin/kubectl (déplacement du binaire dans le PATH)`

`Erreur: 2023-04-23 17:10:45 [✖]  could not find any of the authenticator commands: aws-iam-authenticator, aws`

## Installation de aws cli néccesaire ou aws-iam-authenticator
`curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"`
`unzip awscliv2.zip`
`sudo ./aws/install`

## Création du cluster
`eksctl create cluster --region=ap-southeast-1 --name=cluster-grp3 --nodes=2`

## Vérification de l'état du cluster
`eksctl get cluster`

## Suppression du cluster:
`eksctl delete cluster --name cluster-grp3 --region ap-southeast-1`
