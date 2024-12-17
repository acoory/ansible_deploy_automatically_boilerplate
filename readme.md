# Objectifs du projet
Automatiser le déploiement d'applications à l'aide d'Ansible.
Simplifier la gestion des environnements de développement/production via des rôles Ansible préconfigurés.
Utiliser Docker et Docker Compose pour containeriser les applications et leurs dépendances.
Fournir une infrastructure prête à l'emploi pour les projets Boilerplate avec une architecture modulaire.
Fonctionnalités
Gestion des dépendances : Installation automatique des outils nécessaires comme Docker, Docker Compose, NPM et autres.
Clonage de dépôt : Clonage du dépôt de code source via Git pour une installation rapide.
Déploiement automatique : Lancement des conteneurs Docker avec les configurations requises pour chaque application via Docker Compose.
Support multi-environnement : Possibilité de déployer dans différents environnements (développement, production).

# Guide d'Installation

Ce guide vous explique les étapes nécessaires pour déployer et configurer votre projet avec Ansible et Docker.

## Prérequis

Avant de commencer, vous devez disposer des éléments suivants :

- **Accès SSH** à votre serveur (assurez-vous d'avoir une clé SSH valide)
- **Ansible** installé sur votre machine locale
- **Docker** installé sur le serveur distant (il sera installé via Ansible)
- **Docker Compose** installé sur le serveur distant (il sera également installé via Ansible)

## Étapes d'installation

### 1. Se connecter à votre serveur et copier la clé SSH

Pour vous connecter à votre serveur et copier votre clé SSH, utilisez la commande suivante. Cela permet de configurer l'accès sans mot de passe pour les connexions SSH :

```bash
ssh-copy-id <session_name>@<server_ip>
```

Dans inventories/dev/hosts.ini, ajouter l'adresse IP de votre serveur :

```bash
[dev]
vm ansible_host=<server_ip> ansible_user=<session_name> ansible_ssh_pass=<password>
```

2. exécuter le playbook Ansible

Pour déployer et configurer votre projet, exécutez le playbook Ansible suivant :

```bash
ansible-playbook -i inventories/dev/hosts.ini playbooks/api.yml -kK
```

3. Vérifier le déploiement

Une fois le playbook exécuté, vous pourrez vérifier que l'application est déployée en accédant à l'URL de votre serveur sur le port 3000 :

4. vérifier les logs de l'application docker pour savoir si l'application est bien démarrée et connectée à la base de données :

```bash
sudo docker logs <container_id>
```

Problème rencontré :

- [x] Problème de connexion à la base de données :
  - [x] Solution : Remplacer le nom de l'host docker par l'ip du container de la base de données

Depuis le serveur :

```bash
cd <project_name>
sudo nano .env
```

Remplacer la valeur de `DB_HOST` par l'adresse IP du container de la base de données :

```bash
DB_HOST_DEV=<container_ip>
```

Ensuite, redémarrer le container de l'application :

```bash
sudo docker restart <container_id>
```

5. tester votre première requête

Pour tester votre première requête, vous pouvez utiliser l'outil `postman` pour envoyer une requête POST à l'URL de votre serveur :

```bash
POST http://<server_ip>:3000/api/client/user/create
```
