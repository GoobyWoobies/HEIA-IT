---
description: Ce qu'est une image Docker, comment elle est construite en couches, et comment la gérer.
icon: layer-group
cover: https://placehold.co/1600x500/0f172a/38bdf8?text=Docker+%C2%B7+Les+images
coverY: 0
---

# 3. Les images Docker

<mark style="color:blue;">**Des calques empilés, en lecture seule, partagés entre tous.**</mark>

&#x20;

{% hint style="info" %}
**En bref**

Une image est un **paquet figé** qui contient tout ce qu'il faut pour lancer une application. Elle est faite de **couches** empilées et **partagées** entre images, ce qui économise énormément de place et de temps.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Qu'y a-t-il dans une image ?

&#x20;

```mermaid
flowchart LR
    subgraph IMG["Une image"]
        direction TB
        OS["Un système de fichiers de base<br/>Debian, Alpine…"]
        RT["Un runtime<br/>Node, Python, Java…"]
        DEP["Les dépendances"]
        APP["Le code de l'app"]
        CMD["La commande de démarrage"]
    end

    style IMG fill:#ede9fe,stroke:#8b5cf6,color:#4c1d95
```

&#x20;

{% hint style="success" %}
**Règle de base** — une image est **en lecture seule**. On ne la modifie jamais : pour changer quelque chose, on en **construit une nouvelle**.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">02</mark> · Les couches (layers)

&#x20;

Une image n'est pas un gros bloc : c'est un **empilement de couches** en lecture seule. Chaque couche est une modification par rapport à celle d'en dessous.

&#x20;

```mermaid
flowchart TB
    L4["4dc359259700 · Fichiers de l'application"]
    L3["9977b78fbad7 · Installation d'Apache"]
    L2["e83b3bf07b42 · Mise à jour d'Ubuntu"]
    L1["9cd978db300e · 6170bb7b0ad1 · 51136ea3c5a<br/>Image de base Ubuntu"]
    L4 --- L3 --- L2 --- L1

    style L4 fill:#fef3c7,stroke:#f59e0b,color:#78350f
    style L3 fill:#fee2e2,stroke:#ef4444,color:#7f1d1d
    style L2 fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style L1 fill:#dcfce7,stroke:#22c55e,color:#14532d
```

&#x20;

### L'analogie des calques

&#x20;

Pense aux **calques transparents** d'un logiciel de dessin : chaque calque ajoute quelque chose par-dessus les autres, et l'image finale est la superposition de tous les calques.

&#x20;

### Pourquoi c'est génial ?

&#x20;

Les couches sont **partagées**. Si tu as 10 images basées sur `ubuntu`, la couche Ubuntu n'est stockée et téléchargée **qu'une seule fois**.

&#x20;

```mermaid
flowchart TB
    U[("Couche Ubuntu<br/>stockée une seule fois")]
    U --> A["Image web"]
    U --> B["Image API"]
    U --> C["Image worker"]

    style U fill:#dcfce7,stroke:#22c55e,color:#14532d
    style A fill:#ede9fe,stroke:#8b5cf6,color:#4c1d95
    style B fill:#ede9fe,stroke:#8b5cf6,color:#4c1d95
    style C fill:#ede9fe,stroke:#8b5cf6,color:#4c1d95
```

&#x20;

C'est pour ça que lors d'un `docker pull`, certaines couches sont marquées `Already exists` :

&#x20;

```
$ docker pull ohmyzsh/zsh:latest
latest: Pulling from ohmyzsh/zsh
d7ff0c89abc4: Already exists      ◄── déjà là, pas retéléchargée
58edd5531a03: Already exists
e6db72ea7b7f: Already exists
c820844652a5: Pull complete       ◄── nouvelle couche
4f4fb700ef54: Pull complete
Digest: sha256:6c64ebe0fcc7144a1a105e2526a5448228...
Status: Downloaded newer image for ohmyzsh/zsh:latest
```

&#x20;

***

&#x20;

## <mark style="color:purple;">03</mark> · Nom et tags

&#x20;

Une image s'identifie par un **nom** et un **tag** (une étiquette de version) :

&#x20;

```mermaid
flowchart LR
    R["registry-gitlab.moxoh.ch<br/><i>registry · optionnel</i>"] --> N["moxoh/app<br/><i>nom du dépôt</i>"] --> T["1.4.2<br/><i>tag · version</i>"]

    style R fill:#fef3c7,stroke:#f59e0b,color:#78350f
    style N fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style T fill:#dcfce7,stroke:#22c55e,color:#14532d
```

&#x20;

| Exemple                                  | Signification                                           |
| ---------------------------------------- | ------------------------------------------------------- |
| `nginx`                                  | Image `nginx` sur Docker Hub, tag `latest` implicite     |
| `postgres:16-alpine`                     | PostgreSQL 16, variante Alpine (plus légère)             |
| `traefik:v3.6`                           | Traefik version 3.6                                      |
| `registry.exemple.ch/equipe/app:1.0`     | Image hébergée sur un registry privé                     |

&#x20;

{% hint style="warning" %}
**Le piège du tag `latest`** — `latest` ne veut **pas** dire « la dernière version ». C'est juste le tag par défaut quand on n'en précise aucun. Il peut pointer vers n'importe quoi et changer du jour au lendemain. En production, **fixe toujours une version** (ex. `postgres:16.4`).
{% endhint %}

&#x20;

Une même image peut avoir **plusieurs tags**. Ici, `traefik:3.6` et `traefik:v3.6` ont le même ID :

&#x20;

```
$ docker image ls traefik
IMAGE           ID             DISK USAGE
traefik:3.6     6a74c416e0c4       172MB
traefik:v3.6    6a74c416e0c4       172MB     ◄── même ID = même image
traefik:v3.7    2eb085ca3ba8       175MB
```

&#x20;

***

&#x20;

## <mark style="color:purple;">04</mark> · Les commandes essentielles

&#x20;

{% tabs %}
{% tab title="Lister" %}
```bash
docker image ls           # ou : docker images
docker image ls postgres  # filtrer par nom
```
{% endtab %}

{% tab title="Télécharger" %}
```bash
docker pull nginx                  # depuis Docker Hub
docker pull postgres:16-alpine     # avec un tag précis
```

Depuis un registry privé :

```bash
docker login registry-gitlab.moxoh.ch
docker pull registry-gitlab.moxoh.ch/moxoh/hypotheses/app:latest
```
{% endtab %}

{% tab title="Chercher" %}
```bash
docker search redis
```

Ou directement sur [hub.docker.com](https://hub.docker.com). Privilégie les images avec le badge <mark style="color:green;">**Docker Official Image**</mark> ou <mark style="color:green;">**Verified Publisher**</mark>.
{% endtab %}

{% tab title="Supprimer" %}
```bash
docker image rm nginx     # ou : docker rmi nginx
```
{% endtab %}
{% endtabs %}

&#x20;

### Supprimer une image utilisée

&#x20;

On ne peut supprimer une image que si **aucun conteneur ne l'utilise** :

&#x20;

```
$ docker image rm ohmyzsh/zsh:latest
Error response from daemon: conflict: unable to remove repository reference
"ohmyzsh/zsh:latest" (must force) - container ff680893a575 is using its
referenced image 5443742302f5
```

&#x20;

```mermaid
flowchart TD
    Q{"Un conteneur utilise<br/>l'image ?"}
    Q -->|Non| OK["docker rmi image"]
    Q -->|Oui| A["1. docker rm conteneur"]
    A --> B["2. docker rmi image"]
    Q -.->|"Raccourci déconseillé"| F["docker rmi -f image"]

    style OK fill:#dcfce7,stroke:#22c55e,color:#14532d
    style B fill:#dcfce7,stroke:#22c55e,color:#14532d
    style F fill:#fee2e2,stroke:#ef4444,color:#7f1d1d
```

&#x20;

{% hint style="danger" %}
**`-f` ne fait pas le ménage** — il force la suppression de l'image, mais **le conteneur arrêté n'est pas supprimé** : il reste là, rattaché à une image sans nom. Supprime proprement le conteneur d'abord.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">05</mark> · La couche inscriptible du conteneur

&#x20;

Quand on lance un conteneur, Docker ajoute **une fine couche en lecture/écriture** par-dessus les couches de l'image. Toutes les modifications faites dans le conteneur vont dans cette couche.

&#x20;

```mermaid
flowchart BT
    subgraph IMG["Image ubuntu · lecture seule, partagée"]
        direction BT
        B1["51136ea3c5a"] --- B2["6170bb7b0ad1"] --- B3["9cd978db300e"]
    end
    IMG --> W1["Couche inscriptible<br/>du conteneur 1"]
    IMG --> W2["Couche inscriptible<br/>du conteneur 2"]

    style IMG fill:#dcfce7,stroke:#22c55e,color:#14532d
    style W1 fill:#fef3c7,stroke:#f59e0b,color:#78350f
    style W2 fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
```

&#x20;

C'est ce qui permet de lancer **plusieurs conteneurs depuis la même image** sans dupliquer les données : seule la petite couche du dessus est propre à chacun.

&#x20;

{% hint style="warning" %}
**Attention** — quand un conteneur est **supprimé**, sa couche inscriptible disparaît avec lui, et **toutes les données écrites dedans sont perdues**. Pour les garder, il faut des [volumes](06-volumes.md).
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">06</mark> · En résumé

&#x20;

{% hint style="success" %}
* Une image est un **modèle en lecture seule**, fait de **couches empilées**.
* Les couches sont **partagées** entre images → gain de place et de temps.
* Une image s'identifie par `registry/nom:tag`. Évite `latest` en production.
* Un conteneur = une image + **une couche inscriptible** au-dessus.
{% endhint %}

&#x20;

<mark style="color:green;">**→ Suite :**</mark> [4. Le Dockerfile](04-dockerfile.md)
