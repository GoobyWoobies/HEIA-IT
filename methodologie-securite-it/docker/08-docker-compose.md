---
description: Décrire et lancer une application multi-conteneurs en une seule commande avec Docker Compose.
---

# 8. Docker Compose

## Le problème : trop de conteneurs à gérer

Situation réelle : tu développes une application SaaS pour un client. Son architecture ressemble à ceci :

```
┌───────────────────┐        ┌───────────────────┐        ┌───────────────────┐
│   Web Frontend    │  uses  │    Web Backend    │  uses  │     Database      │
│    (React…)       ├───────►│    (API Node…)    ├───────►│   (PostgreSQL)    │
└───────────────────┘        └───────────────────┘        └───────────────────┘
```

Chaque développeur de l'équipe doit pouvoir lancer **toute la stack** en local. Avec `docker run`, il faudrait :

```bash
docker network create app-net
docker volume create db-data
docker run -d --name db --network app-net -v db-data:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=secret postgres:16
docker build -t backend ./backend
docker run -d --name backend --network app-net -p 3000:3000 \
  -e DATABASE_URL=postgres://postgres:secret@db:5432/postgres backend
docker build -t frontend ./frontend
docker run -d --name frontend --network app-net -p 8080:80 frontend
```

…et s'en souvenir, dans le bon ordre, à chaque fois. 😩 Il nous faut un **mécanisme d'orchestration** : **Docker Compose**.

## La solution : un fichier pour tout décrire

Docker Compose permet de **décrire toute l'application dans un fichier YAML** (`compose.yaml` ou `docker-compose.yml`), puis de la lancer avec **une seule commande**.

### 🎼 L'analogie du chef d'orchestre

* Chaque **conteneur** est un **musicien** : il sait jouer sa partie.
* Le fichier **`compose.yaml`** est la **partition** : il dit qui joue quoi, avec qui, et dans quel ordre.
* **Docker Compose** est le **chef d'orchestre** : il lit la partition et fait jouer tout le monde ensemble.

## Un exemple complet

```yaml
# compose.yaml
services:
  frontend:
    build: ./frontend            # construit l'image depuis ./frontend/Dockerfile
    ports:
      - "8080:80"                # hôte:conteneur
    depends_on:
      - backend                  # démarre après le backend

  backend:
    build: ./backend
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: postgres://postgres:secret@db:5432/postgres
    depends_on:
      - db

  db:
    image: postgres:16           # image prise sur Docker Hub
    environment:
      POSTGRES_PASSWORD: secret
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:                       # volume nommé, géré par Docker
```

Et c'est tout. Pour tout lancer :

```bash
docker compose up
```

### Les éléments clés

| Clé | Rôle |
| --- | --- |
| `services` | La liste des conteneurs de l'application. Chaque service = un conteneur (ou plusieurs répliques) |
| `image` | Image à utiliser (depuis un registry) |
| `build` | Dossier contenant un Dockerfile à construire |
| `ports` | Ports publiés (`hôte:conteneur`), comme `-p` |
| `environment` | Variables d'environnement, comme `-e` |
| `volumes` | Montages de volumes ou bind mounts, comme `-v` |
| `depends_on` | Ordre de démarrage entre services |
| `networks` | Réseaux auxquels le service est connecté |
| `volumes:` (racine) | Déclaration des volumes nommés |
| `networks:` (racine) | Déclaration des réseaux |

{% hint style="success" %}
**La magie du réseau :** Compose crée automatiquement un réseau commun pour tous les services. Chaque service est joignable **par son nom** ! C'est pour ça que le backend peut se connecter à la base via l'hôte `db` (`postgres://…@db:5432/…`) : pas besoin de connaître d'adresse IP.
{% endhint %}

### Séparer les réseaux (plus avancé)

On peut isoler certains services. Ici, le frontend ne peut **pas** parler directement à la base de données : c'est une bonne pratique de sécurité.

```yaml
services:
  frontend:
    image: example/webapp
    ports:
      - "443:8043"
    networks:
      - front-tier
      - back-tier

  backend:
    image: example/database
    volumes:
      - db-data:/etc/data
    networks:
      - back-tier

volumes:
  db-data:

networks:
  front-tier: {}
  back-tier: {}
```

## Les commandes essentielles

| Commande | Effet |
| --- | --- |
| `docker compose up` | Crée et démarre **tous les services** |
| `docker compose up -d` | Idem, en arrière-plan |
| `docker compose up --build` | Reconstruit les images avant de démarrer |
| `docker compose down` | **Arrête et supprime** les conteneurs et réseaux |
| `docker compose down -v` | Idem + supprime les volumes ⚠️ (perte des données) |
| `docker compose ps` | Liste les services et leur état |
| `docker compose logs` | Affiche les logs de tous les services |
| `docker compose logs -f backend` | Suit les logs d'un service en direct |
| `docker compose exec backend sh` | Ouvre un shell dans un service qui tourne |
| `docker compose build` | (Re)construit les images |
| `docker compose restart` | Redémarre les services |

{% hint style="info" %}
Les commandes `docker compose` s'exécutent **dans le dossier qui contient le fichier `compose.yaml`**.
{% endhint %}

{% hint style="warning" %}
Tu verras parfois l'ancienne commande `docker-compose` (avec un tiret). C'était un outil séparé écrit en Python. Aujourd'hui, Compose est intégré à Docker : on écrit **`docker compose`** (avec un espace).
{% endhint %}

## `docker run` vs Docker Compose

| | `docker run` | Docker Compose |
| --- | --- | --- |
| Nombre de conteneurs | Un à la fois | Toute l'application |
| Configuration | Dans la ligne de commande | Dans un fichier versionné avec le code |
| Réseau entre conteneurs | À créer à la main | Automatique, résolution par nom |
| Reproductible par l'équipe | Difficile | `git clone` + `docker compose up` ✅ |

## Pour pratiquer

👉 Suis le tutoriel officiel : [Docker Compose — Getting started](https://docs.docker.com/compose/gettingstarted/)

📖 Référence complète du fichier : [Compose file reference](https://docs.docker.com/reference/compose-file/)

## En résumé

* Docker Compose **décrit une application multi-conteneurs** dans un fichier `compose.yaml`.
* `docker compose up` lance tout, `docker compose down` arrête et nettoie tout.
* Les services se joignent **par leur nom** sur le réseau créé par Compose.
* Le fichier est **versionné avec le code** : toute l'équipe a exactement le même environnement.
