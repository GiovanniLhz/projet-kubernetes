# Journal de Bord Projet Évolutif Sciensano

## Session 3 : Orchestration de Conteneurs avec Kubernetes (MicroK8s)

### 1. Ingestion & Architecture Kubernetes

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

### 3. Résolution d'Incident SysAdmin (Le Piège du Disque & LVM)

- **Diagnostic** : Détection de Pods bloqués en `Pending` à cause d'un taint de sécurité `disk-pressure` (vérifié via `kubectl describe pod`).
- **Ménage système** : Nettoyage du cache et des images inutilisées avec `docker system prune -a -f`.
- **Extension LVM à chaud** : Prise de conscience que l'installateur Ubuntu réserve par défaut la moitié du disque. Utilisation de `lvextend -l +100%FREE` et `resize2fs` pour faire passer la partition principale de 12 Go à 23 Go et retrouver de l'espace.

---

### 4. Validation & Contrôle

- **Application** : `microk8s kubectl apply -f web-deployment.yaml` pour lancer le tout.
- **Vérification CLI** : 
  - `microk8s kubectl get pods` (vérifie que les Pods sont en `Running` à `1/1`).
  - `microk8s kubectl get services` (contrôle le port assigné).
- **Test Navigateur** : Validation visuelle de la page *"Welcome to nginx!"* sur l'hôte via `http://192.168.56.101:30080`.
