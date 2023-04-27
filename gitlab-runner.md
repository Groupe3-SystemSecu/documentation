# GitLab CI

## Installation du runner avec Helm
https://docs.gitlab.com/runner/installj/kubernetes.html

Il faut créer un fichier `runner-chart-values.yaml` avec le contenu suivant
```yaml
gitlabUrl: <gitlab-instance-url>
runnerRegistrationToken: "<token>"
rbac: 
    create: true 
# Read the docs before turning this on: 
# https://docs.gitlab.com/runner/executors/kubernetes.html#using-dockerdind 
runners: 
    privileged: true
```

L'url de notre instance GitLab est `http://18.141.236.117`
Nous avons utilisé le token d'instance pour que le runner soit utilisable par tous les projets.
Cela nous évite d'avoir à enregistrer le runner avec chaque projet.


```shell
helm repo add gitlab https://charts.gitlab.io
helm repo update gitlab
helm install --namespace <namespace> <pod-name> -f <CONFIG_VALUES_FILE> gitlab/gitlab-runner
```
Nous avons utilisé le namespace `gitlab` et le nom de pod `gitlab-runner`

## Installation de l'agent
https://docs.gitlab.com/ee/user/clusters/agent/install/
(à faire dans chaque projet ou bien avec un projet qui gère l'agent et autorise l'accès par les autres)
Dans le projet, aller dans infrastructure -> kubernetes clusters.
Cliquer sur «connect a cluster» et dans la recherche entrer le nom qu'on veut donner à l'agent.
Attention, chaque projet doit avoir un agent avec un nom différent !
Pour la suite, je propose une commande légèrement différente de celle donnée par GitLab.
Elle me semble simplifier un peu la gestion des agents et runners.

```
helm repo add gitlab https://charts.gitlab.io
helm repo update
helm upgrade --install test gitlab/gitlab-agent \
    --namespace <namespace> \
    --create-namespace \
    --set image.tag=v15.10.0 \
    --set config.token=<token> \
    --set config.kasAddress=ws://<gitlab-instance-address>/-/kubernetes-agent/
```

Il *faut* indiquer le token qui nous est donné par GitLab quand on a connecté le cluster
ansi que l'adresse ip de notre instance GitLab (`18.141.236.117`).
Le namespace est optionnel mais nous avons réutilisé le même que pour le runner (`gitlab`)

