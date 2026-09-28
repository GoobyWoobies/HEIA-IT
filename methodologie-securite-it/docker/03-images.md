---
description: Ce qu'est une image Docker, comment elle est construite en couches, et comment la gérer.
---

# 3. Les images Docker

## Qu'est-ce qu'une image ?

Une image est un **paquet figé** qui contient tout le nécessaire pour faire tourner une application :

* un système de fichiers de base (ex. Debian, Alpine) ;
* le runtime (Node.js, Python, Java…) ;
* les librairies et dépendances ;
* le code de l'application ;
* la configuration et la commande à lancer au démarrage.

{% hint style="success" %}
Une image est **en lecture seule**. On ne la modifie jamais : pour changer quelque chose, on en construit une nouvelle.
{% endhint %}

## Les couches (layers)

Une image n'est pas un gros bloc unique : elle est composée d'un **empilement de couches** en lecture seule. Chaque couche représente une modification par rapport à la précédente.

```
   ┌─────────────────────────┐
   │  4dc359259700           │  ◄── Fichiers de l'application
   ├─────────────────────────┤
   │  9977b78fbad7           │  ◄── Installation d'Apache
   ├─────────────────────────┤
   │  e83b3bf07b42           │  ◄── Mise à jour d'Ubuntu
   ├─────────────────────────┤
   │  9cd978db300e           │  ┐
   ├─────────────────────────┤  │
   │  6170bb7b0ad1           │  ├─ Image de base Ubuntu
   ├─────────────────────────┤  │
   │  51136ea3c5a            │  ┘
   └─────────────────────────┘
```

### 🥪 L'analogie du sandwich (ou des calques)

Pense aux **calques transparents** d'un logiciel de dessin : chaque calque ajoute quelque chose par-dessus les autres, et l'image finale est la superposition de tous les calques.

### Pourquoi c'est génial ?

Les couches sont **partagées** entre images. Si tu as 10 images basées sur `ubuntu`, la couche Ubuntu n'est stockée **qu'une seule fois** sur ton disque et téléchargée une seule fois.

C'est pour ça que lors d'un `docker pull`, tu vois certaines couches marquées `Already exists` :

```
$ docker pull ohmyzsh/zsh:latest
latest: Pulling from ohmyzsh/zsh
d7ff0c89abc4: Already exists      ◄── déjà présente localement, pas retéléchargée
58edd5531a03: Already exists
e6db72ea7b7f: Already exists
c820844652a5: Pull complete       ◄── nouvelle couche téléchargée
4f4fb700ef54: Pull complete
a5a42a1464b0: Pull complete
5de55ce99aa0: Pull complete
Digest: sha256:6c64ebe0fcc7144a1a105e2526a5448228...
Status: Downloaded newer image for ohmyzsh/zsh:latest
```

## Nom et tags d'une image

Une image s'identifie par un **nom** et un **tag** (étiquette de version) :

```
   registry-gitlab.moxoh.ch/moxoh/app : 1.4.2
   └──────────┬──────────┘ └───┬───┘   └─┬─┘
         registry            nom        tag
     (optionnel, Docker     (dépôt)   (version)
      Hub par défaut)
```

| Exemple | Signification |
| --- | --- |
| `nginx` | Image `nginx` sur Docker Hub, tag `latest` implicite |
| `postgres:16-alpine` | PostgreSQL 16, variante basée sur Alpine (plus légère) |
| `traefik:v3.6` | Traefik version 3.6 |
| `registry.exemple.ch/equipe/app:1.0` | Image hébergée sur un registry privé |

{% hint style="warning" %}
**Piège du tag `latest` :** `latest` ne veut **pas** dire « la dernière version ». C'est juste le tag par défaut quand on n'en précise aucun. Il peut pointer vers n'importe quelle version et changer du jour au lendemain. En production, **fixe toujours une version précise** (ex. `postgres:16.4`).
{% endhint %}

Un même contenu peut avoir **plusieurs tags**. Ici `traefik:3.6` et `traefik:v3.6` ont le même ID : c'est la même image avec deux étiquettes.

```
$ docker image ls traefik
IMAGE           ID             DISK USAGE
traefik:3.6     6a74c416e0c4       172MB
traefik:v3.6    6a74c416e0c4       172MB     ◄── même ID = même image
traefik:v3.7    2eb085ca3ba8       175MB
```

## Commandes essentielles

### Lister les images locales

```bash
docker image ls          # ou : docker images
docker image ls postgres # filtrer par nom
```

### Télécharger une image

```bash
docker pull nginx                  # depuis Docker Hub
docker pull postgres:16-alpine     # avec un tag précis
```

### Depuis un registry privé

```bash
docker login registry-gitlab.moxoh.ch
docker pull registry-gitlab.moxoh.ch/moxoh/hypotheses/app:latest
```

### Chercher une image

```bash
docker search redis
```

Ou directement sur [hub.docker.com](https://hub.docker.com). Privilégie les images avec le badge **Docker Official Image** ou **Verified Publisher**.

### Supprimer une image

```bash
docker image rm nginx     # ou : docker rmi nginx
```

On ne peut supprimer une image que si **aucun conteneur ne l'utilise** :

```
$ docker image rm ohmyzsh/zsh:latest
Error response from daemon: conflict: unable to remove repository reference
"ohmyzsh/zsh:latest" (must force) - container ff680893a575 is using its
referenced image 5443742302f5
```

Deux solutions :

1. Supprimer d'abord le conteneur (`docker rm ff680893a575`), puis l'image. ✅ **Recommandé**
2. Forcer avec `-f` : `docker image rm -f ohmyzsh/zsh:latest`.

{% hint style="danger" %}
`-f` force la suppression de l'image, mais **le conteneur arrêté n'est pas supprimé** : il reste là, rattaché à une image qui n'a plus de nom. Préfère supprimer proprement le conteneur d'abord.
{% endhint %}

## Du conteneur à l'image : la couche inscriptible

Quand on lance un conteneur, Docker ajoute **une fine couche en lecture/écriture** par-dessus les couches de l'image. Toutes les modifications faites dans le conteneur (fichiers créés, modifiés, supprimés) vont dans cette couche.

```
        IMAGE ubuntu                      CONTENEUR 1                 CONTENEUR 2
                                   ┌──────────────────────┐    ┌──────────────────────┐
                                   │ Couche inscriptible  │    │ Couche inscriptible  │  ◄── propre
                                   ├──────────────────────┤    ├──────────────────────┤      à chaque
┌──────────────────┐   instancie   │ 9cd978db300e         │    │ 9cd978db300e         │      conteneur
│ 9cd978db300e     │ ────────────► │ 6170bb7b0ad1         │    │ 6170bb7b0ad1         │
│ 6170bb7b0ad1     │               │ 51136ea3c5a          │    │ 51136ea3c5a          │  ◄── partagées,
│ 51136ea3c5a      │ ────────────► └──────────────────────┘    └──────────────────────┘      lecture seule
└──────────────────┘
```

C'est ce mécanisme qui permet de lancer **plusieurs conteneurs à partir de la même image** sans dupliquer les données : seule la petite couche du dessus est propre à chacun.

{% hint style="warning" %}
Quand un conteneur est **supprimé**, sa couche inscriptible disparaît avec lui, et **toutes les données écrites dedans sont perdues**. Pour garder des données, il faut utiliser des [volumes](06-volumes.md).
{% endhint %}

## En résumé

* Une image est un **modèle en lecture seule**, composé de **couches empilées**.
* Les couches sont **partagées** entre images → gain de place et de temps.
* Une image s'identifie par `registry/nom:tag`. Évite `latest` en production.
* Un conteneur = image + **une couche inscriptible** au-dessus.
