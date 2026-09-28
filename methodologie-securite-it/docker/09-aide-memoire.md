---
description: Toutes les commandes Docker importantes sur une seule page.
---

# 9. Aide-mémoire

{% hint style="info" %}
Remplace `<image>`, `<conteneur>` et `<volume>` par le nom ou l'ID concerné. Pour un ID, les premiers caractères suffisent s'ils sont uniques.
{% endhint %}

## Images

| Action | Commande |
| --- | --- |
| Construire une image depuis un Dockerfile | `docker build -t <image> .` |
| Construire sans le cache | `docker build -t <image> . --no-cache` |
| Construire en récupérant les dernières images de base | `docker build --pull -t <image> .` |
| Lister les images locales | `docker images` ou `docker image ls` |
| Supprimer une image | `docker rmi <image>` ou `docker image rm <image>` |
| Supprimer les images inutilisées | `docker image prune` |

## Docker Hub / registry

| Action | Commande |
| --- | --- |
| Se connecter | `docker login -u <utilisateur>` |
| Se connecter à un registry privé | `docker login <registry>` |
| Rechercher une image | `docker search <image>` |
| Télécharger une image | `docker pull <image>` |
| Publier une image | `docker push <utilisateur>/<image>` |

## Conteneurs

| Action | Commande |
| --- | --- |
| Lancer un conteneur avec un nom | `docker run --name <conteneur> <image>` |
| Lancer en mode interactif | `docker run -it <image> sh` |
| Lancer en arrière-plan | `docker run -d <image>` |
| Publier un port | `docker run -p <port_hôte>:<port_conteneur> <image>` |
| Passer une variable d'environnement | `docker run -e CLE=valeur <image>` |
| Supprimer automatiquement à l'arrêt | `docker run --rm <image>` |
| Démarrer / arrêter | `docker start <conteneur>` / `docker stop <conteneur>` |
| Lister les conteneurs en cours | `docker ps` |
| Lister tous les conteneurs | `docker ps -a` |
| Supprimer un conteneur arrêté | `docker rm <conteneur>` |
| Supprimer tous les conteneurs arrêtés | `docker container prune` |
| Ouvrir un shell dans un conteneur qui tourne | `docker exec -it <conteneur> sh` |
| Voir et suivre les logs | `docker logs -f <conteneur>` |
| Inspecter la configuration | `docker inspect <conteneur>` |
| Voir la consommation CPU / RAM | `docker container stats` |

## Volumes et montages

| Action | Commande |
| --- | --- |
| Créer un volume | `docker volume create <volume>` |
| Lister les volumes | `docker volume ls` |
| Supprimer un volume | `docker volume rm <volume>` |
| Monter un volume | `docker run -v <volume>:/chemin <image>` |
| Monter un dossier de l'hôte (bind mount) | `docker run -v /chemin/hôte:/chemin <image>` |
| Monter un tmpfs | `docker run --mount type=tmpfs,destination=/chemin <image>` |

## Maintenance

| Action | Commande |
| --- | --- |
| Espace disque utilisé | `docker system df` |
| Nettoyage général | `docker system prune` |
| Nettoyage incluant les volumes | `docker system prune --volumes` |
| Infos sur l'installation | `docker system info` |

## Docker Compose

| Action | Commande |
| --- | --- |
| Démarrer tous les services | `docker compose up` (`-d` en arrière-plan) |
| Reconstruire et démarrer | `docker compose up --build` |
| Arrêter et supprimer | `docker compose down` (`-v` pour les volumes) |
| État des services | `docker compose ps` |
| Logs | `docker compose logs -f` |
| Shell dans un service | `docker compose exec <service> sh` |

## Modèle de Dockerfile

```dockerfile
FROM <image_de_base>:<version>
WORKDIR /app
COPY <fichiers_de_dépendances> ./
RUN <installation_des_dépendances>
COPY . .
EXPOSE <port>
CMD ["<commande>", "<argument>"]
```

## Modèle de compose.yaml

```yaml
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      DB_HOST: db
    depends_on:
      - db
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:
```

📄 Cheat sheet officielle : [docker_cheatsheet.pdf](https://docs.docker.com/get-started/docker_cheatsheet.pdf)
