# Journal d'explication pour  le Projet sur kubernetes 


### 1. Initialisation & Architecture du Cluster

- **Déploiement d'un cluster Kubernetes léger via MicroK8s** sur l'environnement Ubuntu Server (`192.168.56.101`).
- **Prise en main et administration** des objets du cluster via l'outil en ligne de commande `kubectl`.
- **Configuration par fichier YAML** pour gérer séparément les conteneurs Nginx et leur port d'accès réseau.

---

### 2. Rédaction du Fichier de Configuration (`web-deployment.yaml`)

- **Déploiement (Deployment) :**
  - **Normalisation du nom** : Nommé `mon-site-web-deployment` au format kebab-case.
  - **`replicas: 2`** : Configuration de 2 copies identiques pour assurer la haute disponibilité.
  - **`image: nginx:latest`** : Définition du conteneur basé sur l'image officielle Nginx téléchargée depuis le site officiel avec ses dépendances.
  - **`containerPort: 80`** : Ouverture du port interne où Nginx écoute.

- **Service Réseau (Service - NodePort) :**
  - **`kind: Service`** : Création d'un point d'entrée réseau (un standardiste).
  - **`type: NodePort`** : Ouvre une porte externe sur la VM pour intercepter les requêtes.
  - **`selector:` / `app: mon-site-web`** : Permet au service de reconnaître et cibler les bons Pods grâce à leur badge.
  - **Mappage des ports** : 
    - `port: 80` (port du service interne)
    - `targetPort: 80` (redirection vers le conteneur)
    - `nodePort: 30080` (le port externe accessible via `http://192.168.56.101:30080`)

---

### 4. Validation & Contrôle

- **Application** : `microk8s kubectl apply -f web-deployment.yaml` pour lancer le tout.
- **Vérification CLI** : 
  - `microk8s kubectl get pods` (vérifie que les Pods sont en `Running` à `1/1`).
  - `microk8s kubectl get services` (contrôle le port assigné).
- **Test Navigateur** : Validation visuelle de la page *"Welcome to nginx!"* sur l'hôte via `http://192.168.56.101:30080`.
