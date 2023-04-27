# Documentation de l'infrastructure

Ce dépôt présente la documentation de l'infrastructure mise en place.

Tout d'abord, nous utilisons un cluster EKS pour le runner GitLab CI/CD et les containers de production.
La procédure utilisée pour son installation peut être trouvée dans le fichier [eks.md](eks.md).
Les informations d'installation du runner dans kubernetes sont trouvables dans [gitlab-runner.md](gitlab-runner.md).

Le serveur GitLab est installé dans sa propre instance EC2 avec la procédure décrite dans le fichier [gitlab.md](gitlab.md).

La documentation des différents projets est trouvable dans les fichiers suivants :
 - [application de vote](app-vote.md)
 - [site vétérinaire](app-vet.md)
 - [site en python](app-python.md)
