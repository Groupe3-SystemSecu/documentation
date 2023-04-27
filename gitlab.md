%title: Documentation gitlab

## Première partie : installation d'un serveur debian sur aws.

*- Choix du type d'instance : c4xLarge permettant d'avoir suffisamment de puissance pour accueillir gitlab.
*- Connexion au serveur grâce à cette commande ssh: 

'''
ssh -i "SANDSLASH.pem" admin@18.141.236.117
'''


## Deuxième partie : installation de docker grâce à ce script:

'''
apt-get update
apt-get install -y
ca-certificates
curl
gnupg
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
chmod a+r /etc/apt/keyrings/docker.gpg
echo
"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian
"$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" |
tee /etc/apt/sources.list.d/docker.list > /dev/null
apt-get update
apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
'''


## Troisième partie : déploiement de l'installation docker grâce à la deuxième partie du script:

'''
mkdir /srv/gitlab && mkdir /srv/gitlab/{data,logs,config}
export GITLAB_HOME=/srv/gitlab
docker run --detach
--hostname gitlab.local
--publish 80:80
--name gitlab
--restart always
--volume $GITLAB_HOME/config:/etc/gitlab
--volume $GITLAB_HOME/logs:/var/log/gitlab
--volume $GITLAB_HOME/data:/var/opt/gitlab
--shm-size 256m
gitlab/gitlab-ce:latest
'''


* Pour trouver le mot de passe administrateur de gitlab généré à l'installation:

'''
cat /srv/gitlab/config/initial_root_password
'''

* Pour trouver le token d'intégration nécssaire pour l'intégraion continue pour kubernetes : 
'''
docker exec -it 13ffa82acdd9 /bin/bash gitlab-rails runner "puts Gitlab::CurrentSettings.current_application_settings.runners_registration_token"
'''

* Commande pour créer des utilisateurs en ligne de commandes, toujpurs pour l'intégration continue:
'''
exec -it 13ffa82acdd9 /bin/bash gitlab-rails runner "user = User.create(name: 'totodeux', username: 'totodeux', email: 'totodeux@grp3.local', password: 'azerty75', password_confirmation: 'azerty75'); user.save!"
'''