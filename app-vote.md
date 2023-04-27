Pour déployer l'application, dans le dossier projet-nodejs exécuter la commande suivante : 
`kubectl create -f k8s-specifications/`
Pour isoler les différentes applications on peut utiliser des namespaces avec `--namespace voting-app` par exemple.

Le group de sécurité par défaut n’autorise aucune connexion entrante. Il faut le modifier pour ajouter des règles correspondant à nos services.

Ajouter les groupes de sécurité sur le cluster (TCP personnalisé : ports 31000 et 310001 pour l'appli voting-app) pour pouvoir accéder aux sites sur les nodes créés par EKS à partir de leur adresse IP.
(on peut utiliser le noeud que l'on veut + le port) 
