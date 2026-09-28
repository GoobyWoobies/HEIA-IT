---
description: Comment Docker fonctionne en coulisses — client, daemon, registry et objets Docker.
---

# 2. Architecture de Docker

Quand tu tapes `docker run nginx`, plusieurs acteurs travaillent ensemble. Comprendre qui fait quoi rend Docker beaucoup moins mystérieux.

## Les trois composants

### 1. Le client Docker

C'est **l'interface que tu utilises** : la commande `docker` dans ton terminal. Il ne fait rien lui-même : il **transmet tes ordres** au daemon et t'affiche les réponses.

### 2. Le daemon Docker (`dockerd`)

C'est **le moteur qui fait le vrai travail**. Il tourne en arrière-plan sur la machine hôte et gère les images, les conteneurs, les réseaux et les volumes. Tu n'interagis jamais directement avec lui : tu passes par le client, qui lui parle via une **API REST** (à travers un socket Unix ou le réseau).

### 3. Le registry (Docker Hub)

C'est une **bibliothèque d'images** hébergée en ligne. Le registry public par défaut est [Docker Hub](https://hub.docker.com), qui contient des millions d'images (Ubuntu, Nginx, PostgreSQL, Node…). Les entreprises ont souvent leur propre registry privé (GitLab, GitHub, Harbor…).

### 🍽️ L'analogie du restaurant

| Restaurant | Docker |
| --- | --- |
| Toi, le client | Toi, dans le terminal |
| Le serveur qui prend la commande | Le **client** Docker (`docker`) |
| La cuisine qui prépare les plats | Le **daemon** Docker (`dockerd`) |
| Le fournisseur de produits | Le **registry** (Docker Hub) |
| La recette | L'**image** |
| Le plat servi | Le **conteneur** |

Tu ne vas jamais en cuisine : tu passes par le serveur. Et si la cuisine n'a pas un ingrédient, elle le commande au fournisseur.

## Le schéma complet

```
   CLIENT                    DOCKER_HOST                        REGISTRY
┌──────────────┐   ┌──────────────────────────────────┐   ┌──────────────────┐
│              │   │          Docker daemon            │   │                  │
│ docker build ├──►│                                   │   │  ubuntu   nginx  │
│              │   │  ┌────────────┐   ┌────────────┐  │   │                  │
│ docker pull  ├──►│  │ Containers │   │   Images   │◄─┼───┤  redis  postgres │
│              │   │  │            │   │            │  │   │                  │
│ docker run   ├──►│  │  ▣ ▣ ▣ ▣   │◄──┤  ubuntu    │  │   │   (Docker Hub)   │
│              │   │  │            │   │  redis     │  │   │                  │
└──────────────┘   │  └────────────┘   └────────────┘  │   └──────────────────┘
                   └──────────────────────────────────┘
```

Que se passe-t-il pour chaque commande ?

{% tabs %}
{% tab title="docker pull" %}
1. Le client demande au daemon de récupérer une image.
2. Le daemon **télécharge l'image depuis le registry**.
3. L'image est stockée localement, prête à l'emploi.
{% endtab %}

{% tab title="docker build" %}
1. Le client envoie au daemon un `Dockerfile` et les fichiers du projet (le _build context_).
2. Le daemon **construit une nouvelle image** en suivant les instructions du Dockerfile.
3. L'image est stockée localement.
{% endtab %}

{% tab title="docker run" %}
1. Le client demande au daemon de lancer un conteneur à partir d'une image.
2. Si l'image **n'existe pas localement**, le daemon la **télécharge** d'abord depuis le registry (pull automatique).
3. Le daemon **crée et démarre le conteneur**.
{% endtab %}
{% endtabs %}

## Les objets Docker

Docker manipule plusieurs types d'objets. Les deux plus importants sont les **images** et les **conteneurs**.

### Image

* Un **modèle en lecture seule** contenant les instructions pour créer un conteneur.
* Souvent **basée sur une autre image**, avec des personnalisations en plus.
* _Exemple :_ une image basée sur `ubuntu`, qui installe le serveur web Apache, ton application et sa configuration.

### Conteneur

* Une **instance exécutable d'une image**.
* On peut le démarrer, l'arrêter, le déplacer, le supprimer.
* On peut le connecter à des réseaux, lui attacher du stockage, ou même créer une nouvelle image à partir de son état actuel.

### 🧁 L'analogie du moule à gâteau

> L'**image** est le **moule** : il ne change jamais.
> Le **conteneur** est le **gâteau** : tu peux en faire autant que tu veux avec le même moule, et chacun peut ensuite être décoré différemment.

Ou, pour les développeurs : **une image est à un conteneur ce qu'une classe est à un objet.**

```
                    ┌────────────────┐
                    │  IMAGE nginx   │   (lecture seule, une seule copie)
                    └───────┬────────┘
            ┌───────────────┼───────────────┐
            ▼               ▼               ▼
     ┌────────────┐  ┌────────────┐  ┌────────────┐
     │ conteneur  │  │ conteneur  │  │ conteneur  │   (instances indépendantes)
     │  site-a    │  │  site-b    │  │  test      │
     └────────────┘  └────────────┘  └────────────┘
```

{% hint style="info" %}
Les autres objets Docker sont les **volumes** (stockage persistant, voir [chapitre 6](06-volumes.md)) et les **réseaux** (communication entre conteneurs, voir [Docker Compose](08-docker-compose.md)).
{% endhint %}

## En résumé

* Le **client** (`docker`) envoie tes commandes au **daemon** (`dockerd`), qui fait le travail.
* Le **registry** (Docker Hub) stocke et distribue les images.
* Une **image** est un modèle figé ; un **conteneur** est une instance vivante de cette image.
