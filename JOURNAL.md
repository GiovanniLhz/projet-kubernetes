# Journal de Bord Projet Évolutif Sciensano

## Session 3 : Orchestration de Conteneurs avec Kubernetes (MicroK8s)

### 1. Ingestion & Architecture Kubernetes

* Déploiement d'un cluster Kubernetes léger via **MicroK8s** sur l'environnement Ubuntu Server (`192.168.56.101`).
* Prise en main et administration des objets du cluster via l'outil en ligne de commande **kubectl**.
* Configuration par fichier YAML pour gérer séparément les conteneurs Nginx et leur port d'accès réseau.

### 2. Rédaction du Fichier de Configuration (`web-deployment.yaml`)

* **Déploiement (Deployment) :**
  * Normalisation du nommage au format kebab-case (`mon-site-web-deployment`).
  * Configuration de **2 réplicas** (`replicas: 2`) pour assurer la haute disponibilité.
  * Définition du conteneur basé sur l'image officielle **Nginx** (`nginx:latest`) avec ouverture du port interne `80`.
* **Service Réseau (Service - NodePort) :**
  * Création d'un service de type **NodePort** (`mon-site-web-service`).
  * Liaison du service aux Pods via le sélecteur d'étiquettes (`app: mon-site-web`).
  * Mappage des ports : exposition externe sur le port **30080** redirigeant vers le port **80** du service et des Pods.

### 3. Gestion de Version & Publication GitHub

* Initialisation du dépôt Git local et suivi du fichier de configuration.
* Publication directe de la branche master sur un dépôt distant **GitHub** public (`projet-kubernetes`).
* Récupération du dépôt sur la machine virtuelle distante via **SSH** avec `git clone`.

### 4. Validation & Contrôle Qualité

* Application du fichier de configuration sur le cluster via `microk8s kubectl apply -f web-deployment.yaml`.
* **Vérification CLI :**
  * Validation du démarrage et de l'état des Pods via `microk8s kubectl get pods`.
  * Contrôle de la création et du port attribué via `microk8s kubectl get services`.
* **Rendu Navigateur :** Validation visuelle du serveur Nginx sur l'hôte via `http://192.168.56.101:30080`.