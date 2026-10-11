# Déploiement Nginx sur Kubernetes (MicroK8s)

Ce projet déploie un serveur web Nginx sur un cluster Kubernetes léger (MicroK8s), tournant sur une VM Ubuntu Server. C'est la troisième étape de mon projet évolutif : après Ansible et Docker, j'ai voulu voir comment Kubernetes gère les conteneurs à ma place. Le détail de ma démarche est dans [JOURNAL.md](JOURNAL.md).

## Objectif

Lancer deux copies (réplicas) d'un serveur Nginx, les exposer vers l'extérieur avec un service NodePort, et vérifier que le site répond depuis le navigateur.

## Fichiers

- `web-deployment.yaml` : contient le Deployment (les 2 réplicas Nginx) et le Service (accès réseau sur le port 30080)
- `JOURNAL.md` : mon journal de bord

## Environnement

- MicroK8s sur une VM Ubuntu Server (192.168.56.101)
- Gestion du cluster avec `kubectl` (fourni avec MicroK8s)

## Lancer le projet

Appliquer la configuration sur le cluster :

```bash
microk8s kubectl apply -f web-deployment.yaml
```

Vérifier que les Pods tournent :

```bash
microk8s kubectl get pods
```

Vérifier le service et le port attribué :

```bash
microk8s kubectl get services
```

Tester le résultat dans le navigateur :
