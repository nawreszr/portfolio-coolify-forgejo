# Rapport TP2 : Coolify et Forgejo

## 1. Retour sur le déploiement
Le déploiement de l'application Next.js via Coolify est un succès. L'application est accessible sur `http://portfolio.coolify.local`. Le pipeline CI/CD est entièrement automatisé : un `git push` sur Forgejo déclenche un webhook qui ordonne à Coolify de reconstruire et redéployer l'application.

## 2. Tableau comparatif : Coolify (PaaS) vs Render (PaaS managé)
| Critère | Coolify (TP2) | Render (TP3) |
|---|---|---|
| **Hébergement** | Auto-hébergé (sur notre propre VM) | Managé (Cloud de Render) |
| **Gestion du serveur** | C'est nous qui gérons la VM, Docker, le proxy | Zéro gestion serveur, tout est abstrait |
| **Intégration Git** | Via un webhook Forgejo (auto-hébergé) | Via l'application GitHub native |
| **Coût** | Gratuit (hors coût de la VM) | Modèle Freemium (payant pour la prod) |
| **Contrôle** | Total (on peut modifier le proxy, accéder au serveur) | Limité aux options de l'interface Render |

## 3. Difficultés rencontrées
- **Espace disque** : L'installation de Coolify a échoué au début à cause d'un disque trop petit (10 Go). Résolu en agrandissant le disque virtuel à 30 Go via LVM.
- **Build Next.js** : Coolify a utilisé Node.js 18 par défaut, causant une erreur de build. Résolu en ajoutant la variable `NIXPACKS_NODE_VERSION=22` dans les paramètres de l'application.
- **Routage Traefik** : Erreur 404 au début. Résolu en forçant le domaine `http://portfolio.coolify.local` et en redémarrant le proxy de Coolify.
- **Syntaxe du code** : Une erreur de commentaire HTML (`<!-- -->`) dans un fichier `.tsx` a fait échouer un build. Résolu en utilisant une syntaxe JavaScript valide (`//`).
