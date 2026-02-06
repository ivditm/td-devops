# TP Docker Hub

## 1. Historique de Docker Hub

- **2013** : Lancé peu après Docker lui-même  
- Première plateforme grand public pour **stocker** et **partager** des images de conteneurs  
- A largement contribué à la popularisation de la conteneurisation dans l’industrie

---

## 2. Fonctionnalités de Docker Hub

- **Création de dépôts publics ou privés**  
- **Gestion des tags** pour versionner vos images  
- **Automated builds** (sur GitHub/Bitbucket)  
- **Scans de vulnérabilités** (fonctionnalité payante ou tierce)  
- **Équipes et organisations** pour la collaboration

---

## 3. Création d’un dépôt sur Docker Hub

1. **Inscription** sur [hub.docker.com](https://hub.docker.com/)  
2. **Créer un repository**  
   - Choisir `Public` ou `Private`  
   - Nom et description  
3. **Configurer la visibilité** :  
   - Public : toute la communauté peut pull l’image  
   - Privé : accessible seulement à des collaborateurs autorisé

---

## 4. Pusher une image vers Docker Hub

1. **Se connecter** à Docker Hub en ligne de commande  

   ```bash
   docker login
   # Entrez votre username et mot de passe Docker Hub
   ```

2. **Tagger l’image** localement  

   ```bash
   docker tag mon-appli:latest monuser/mon-appli:1.0
   ```

3. **Push sur Docker Hub**  

   ```bash
   docker push monuser/mon-appli:1.0
   ```

   - L’image est alors disponible en téléchargement (`docker pull`)

---

## 5. Alternatives à Docker Hub

- **GitLab Container Registry**  
  - Avantage : intégré à GitLab, CI/CD simplifiée  
- **Harbor**  
  - Auto-hébergé, fonctionnalités avancées (scan, signature)  
- **Amazon ECR**, **Google Container Registry**, **Azure Container Registry**  
  - Parfaites pour un écosystème 100% Cloud  
- **GitHub Packages**  
  - Intégré dans GitHub, utilise la syntaxe `docker pull ghcr.io/...`

---

## Conclusion du TP Docker Hub

- Docker Hub reste un point d’entrée **simple et pratique** pour héberger et partager vos images.  
- Le **push/pull** se fait facilement avec des commandes Docker standard.  
- Des alternatives existent selon votre **infrastructure**, votre **budget** ou vos **besoins en sécurité** (public vs privé).  

---
