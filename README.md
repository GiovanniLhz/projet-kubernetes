# Déploiement Nginx sur Kubernetes (MicroK8s)

L’objectif de ce projet est de déployer un serveur web Nginx sur ma VM Ubuntu grâce à MicroK8s, une version légère de Kubernetes. Contrairement à Docker qui gère des conteneurs simples, Kubernetes déploie ici 2 réplicas (copies )de mon application sous forme de Pods. Si un Pod plante ou doit être mis à jour, Kubernetes le remplace automatiquement, sans coupure de service. L’application est rendue accessible depuis l’extérieur sur le port 30080 de la VM, grâce à un service de type NodePort. 

Le détail de ma démarche est dans [JOURNAL.md](JOURNAL.md).


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
Rendez-vous sur `http://<IP-DE-LA-VM>:30080` (ex: `http://192.168.56.101:30080` sur un réseau Host-Only VirtualBox) pour afficher la page par défaut de Nginx.
