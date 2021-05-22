ANSIBLE
========

#### Version 

```
$ ansible --version
ansible 2.9.7
```

## Commandes Ad-hoc :


- [ ] Créer un Cluster (1 ansible 1 client)
- [ ] Créer un fichier d'inventaire
- [ ] Utiliser commandes ad-hoc comme pour pinger le client
- [ ] Tester le module Setup


- Inventaire (hosts) :
```
10.0.0.3 ansible_user=vagrant ansible_password=vagrant ansible_ssh_common_args='-o StrictHostKeyChecking=no'
10.0.0.4 ansible_user=vagrant ansible_password=vagrant ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```
- Module ping :

     `$ ansible -i hosts all -m ping`
 
- Module copy :

    `$ ansible -i hosts all -m copy -a "dest=/home/vagrant/tata.txt content='Bonjour Adama'"`

-  Module Setup

        `$ ansible -i hosts all -m setup`
Ce module pourrait permettre d'avoir toute information sur le client


## Inventaire au format YAML

```
all:
  hosts:
    10.0.0.3:
      ansible_user: vagrant
      ansible_password: vagrant
      ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
    10.0.0.4:
      ansible_user: vagrant
      ansible_password: vagrant
      ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
    10.0.0.5:
      ansible_user: vagrant
      ansible_password: vagrant
      ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
```

Exple de Besoins :

On a plusieurs types de Serveurs au sein de notre DSI ( Serveur *WEB*, Serveur *APP* et Serveur *DB*) et chaque serveur a un rôle spécifique.

#### Structure INI
```
[all:vars]
ansible_user=vagrant
ansible_ssh_common_args='-o StrictHostKeyChecking=no'

[prod]
node1 ansible_host=10.0.0.3
#10.0.0.3

[prod:vars]
ansible_password=vagrant
env=production
```

Commandes :

```
ansible -i hosts.ini all -m ping
ansible -i hosts.ini prod -m ping
ansible -i hosts.ini node1 -m ping
```

Utiliser une *Variable*

`$ ansible -i hosts.ini all -m copy -a "dest=/home/vagrant/tata.txt content='Bonjour Adama - Environnement de {{ env }}'"`

#### Structure YAML

```
all:
  vars:
    ansible_user: vagrant
    ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
prod:
  hosts:
    node3:
      ansible_host: 10.0.0.5
    node2:
      ansible_host: 10.0.0.4
  vars:
    ansible_password: vagrant
    env: production

```

Commande:

`ansible -i hosts.yml prod -m copy -a "dest=/home/vagrant/tata.txt content='Ceci est un Environnement de {{ env }}'"`


### Playbook

Si nous avons plusieurs actions à faire sur un serveur, i faudrait mieux utiliser les Playbook.

- [x] Surcharge des Hosts et Group vars
- [x] Variables à partir d'un fichier
- [x] Inventaire par environnement
- [x] Un Playbook pour chaque grande action à faire

- *Créer le fichier hosts.yml*
```
all:
  vars:
    ansible_ssh_common_args: '-o StrictHostKeyChecking=no'
prod:
  hosts:
    node1:
      ansible_host: 10.0.0.1
    node2:
      ansible_host: 10.0.0.2
```

- *Créer le Group vars*

```
mkdir groups_vars
vim groups_vars/prod.yml
```
contenu :
```
---
ansible_user: vagrant
ansible_password: vagrant
```

- *Lancer le playbook*
- [] deploy.yml
```
---
- name: "Apache Installation using Docker"
  hosts: prod
  tasks:
  - name: Create Apache container
    docker_container:
      name: webapp
      image: apache2
      ports:
        - "80:80"
```

- [] *Checker la styntaxe du Playbook avant l'install*

```
pip3 install ansible-lint
ansible-lint deploy.yml
```

### Templating, Loop, Condition

### Sécurité

### Role ansible

### Mini-projet


- [ ] Création de [compte AWS](https://aws.amazon.com/fr/premiumsupport/knowledge-center/create-and-activate-aws-account/)
    - [ ] Nom compte : diengitdevops
    - [ ] Zone : Virginie Nord
    - [ ] Créer un Compte nominatif (adama | AdministratorAccess  | )
    - [ ] Alias de connexion AWS : diengdevops
    - [ ] 