# Hébergement local avec Docker

Ce dépôt contient la configuration d’hébergement. Le code du site web est maintenu séparément dans le dépôt privé [`test-IA`](https://github.com/artos456/test-IA).

## Organisation

- `docker-compose.yml` : services et ports
- `nginx/default.conf` : routage web
- `site/` : contenu statique éventuellement servi directement
- `apps/` : applications de démonstration PHP, Python, Node.js et Java
- `.env.example` : modèle de configuration SQL (ne pas y mettre de secrets)

Le point d’entrée web est Nginx sur le port hôte **8205**. Les services applicatifs et MariaDB ne publient pas leurs ports sur l’hôte.

## Relier le site `test-IA`

Le serveur doit avoir accès au dépôt privé `test-IA` (par exemple via une clé de déploiement SSH en lecture seule). Clonez le dépôt du site dans `./site` pour un site HTML/CSS/JS statique, ou montez-le dans le service applicatif adapté si le projet utilise PHP, Node.js, Python ou Java. Ne placez jamais de jeton GitHub ou de mot de passe dans un fichier suivi par Git.

Après synchronisation des fichiers du site, Nginx les sert sur `http://localhost:8205/`. Le déploiement automatique à chaque push nécessite une étape supplémentaire côté serveur (webhook sécurisé ou tâche de synchronisation).

## Démarrage

1. Préparer les fichiers de configuration et les exemples applicatifs selon l’arborescence ci-dessus.
2. Copier `.env.example` vers `.env` et définir des mots de passe robustes.
3. Vérifier et démarrer :

```sh
docker compose config
docker compose up -d
```

La configuration complète ainsi que les fichiers d’exemple sont à ajouter dans ce dépôt avant lancement. N’exposez pas directement MariaDB sur Internet; ajoutez TLS et une configuration de sécurité adaptée avant toute exposition publique.
