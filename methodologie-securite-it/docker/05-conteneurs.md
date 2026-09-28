---
description: Lancer, arrêter, supprimer et interagir avec des conteneurs, et publier leurs ports.
icon: play
cover: https://placehold.co/1600x500/0f172a/38bdf8?text=Docker+%C2%B7+Les+conteneurs
coverY: 0
---

# 5. Les conteneurs

<mark style="color:blue;">**Une image qui prend vie.**</mark>

&#x20;

{% hint style="info" %}
**En bref**

`docker run` crée et démarre un conteneur. On l'arrête avec `stop`, on le supprime avec `rm`, on entre dedans avec `exec`, et on le rend accessible avec `-p`.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Lancer un conteneur

&#x20;

```bash
docker run ohmyzsh/zsh
```

&#x20;

`docker run` fait en réalité **trois choses d'un coup** :

&#x20;

```mermaid
flowchart LR
    R(["docker run"]) --> P["📥 1. pull<br/>si l'image manque"]
    P --> C["🧱 2. create<br/>crée le conteneur"]
    C --> S["▶️ 3. start<br/>lance CMD"]

    style R fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style P fill:#fef3c7,stroke:#f59e0b,color:#78350f
    style C fill:#ede9fe,stroke:#8b5cf6,color:#4c1d95
    style S fill:#dcfce7,stroke:#22c55e,color:#14532d
```

&#x20;

{% hint style="info" %}
Un conteneur vit **aussi longtemps que son processus principal**. Quand la commande se termine, le conteneur s'arrête. C'est pour ça que `docker run ohmyzsh/zsh` rend la main tout de suite : sans terminal attaché, le shell n'a rien à faire et quitte.
{% endhint %}

&#x20;

### Les options à connaître

&#x20;

| Option              | Effet                                                                  | Exemple                          |
| ------------------- | ---------------------------------------------------------------------- | -------------------------------- |
| `-it`               | Mode **interactif** : ton clavier et ton écran sont branchés dessus     | `docker run -it ubuntu bash`     |
| `--name`            | Donne un **nom** (sinon Docker invente, ex. `condescending_tharp`)      | `--name my-server`               |
| `-d`                | Mode **détaché** : tourne en arrière-plan                               | `docker run -d nginx`            |
| `-p hôte:conteneur` | **Publie un port**                                                      | `-p 8080:80`                     |
| `-e`                | Définit une **variable d'environnement**                                | `-e POSTGRES_PASSWORD=secret`    |
| `-v`                | Monte un **volume** (voir [chapitre 6](06-volumes.md))                  | `-v data:/var/lib/data`          |
| `--rm`              | **Supprime automatiquement** le conteneur à l'arrêt                     | `docker run --rm -it alpine sh`  |

&#x20;

{% tabs %}
{% tab title="💬 Shell interactif" %}
```bash
docker run -it --name my-zsh ohmyzsh/zsh
```
{% endtab %}

{% tab title="🌐 Serveur en fond" %}
```bash
docker run -d --name my-server -p 8080:80 nginx
```
{% endtab %}

{% tab title="🧪 Test jetable" %}
```bash
docker run --rm -it alpine sh
```

Parfait pour tester quelque chose : le conteneur disparaît dès que tu quittes.
{% endtab %}
{% endtabs %}

&#x20;

***

&#x20;

## <mark style="color:purple;">02</mark> · Lister, arrêter, supprimer

&#x20;

```bash
docker ps            # conteneurs EN COURS  (= docker container ls)
docker ps -a         # TOUS, y compris arrêtés
```

&#x20;

```
$ docker ps -a
CONTAINER ID   IMAGE   STATUS                     PORTS    NAMES
3f72126a581a   nginx   Up 48 seconds              80/tcp   my-server2
6fd0cc9552ea   nginx   Exited (0) 2 minutes ago            my-server
```

&#x20;

{% columns %}
{% column %}
### ⏹️ Arrêter / relancer

```bash
docker stop my-server
docker start my-server
docker restart my-server
```

Par nom ou par ID (les premiers caractères suffisent).
{% endcolumn %}

{% column %}
### 🗑️ Supprimer

```bash
docker rm my-server
docker rm 16aea41c1946

# tous les conteneurs arrêtés
docker container prune
```
{% endcolumn %}
{% endcolumns %}

&#x20;

{% hint style="warning" %}
**Arrêté ≠ supprimé** — un conteneur stoppé existe toujours (visible avec `docker ps -a`) et **occupe de l'espace disque**. Pense à faire le ménage.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">03</mark> · Le cycle de vie

&#x20;

```mermaid
stateDiagram-v2
    direction LR
    [*] --> Created : create
    [*] --> Running : run
    Created --> Running : start
    Running --> Paused : pause
    Paused --> Running : unpause
    Running --> Stopped : stop
    Stopped --> Running : start
    Created --> Deleted : rm
    Stopped --> Deleted : rm
    Deleted --> [*]
```

&#x20;

| État                  | Description                                    | Commande          |
| --------------------- | ---------------------------------------------- | ----------------- |
| ⚪ **Created**        | Créé, jamais démarré                           | `docker create`   |
| 🟢 **Running**        | En cours d'exécution                           | `docker start` / `run` |
| 🟡 **Paused**         | Processus gelés en mémoire                     | `docker pause`    |
| 🔴 **Stopped**        | <mark style="color:orange;">Arrêté, mais encore sur le disque</mark> | `docker stop`     |
| ⚫ **Deleted**        | Supprimé définitivement                        | `docker rm`       |

&#x20;

***

&#x20;

## <mark style="color:purple;">04</mark> · Entrer dans un conteneur : `exec`

&#x20;

`docker exec` lance une commande **supplémentaire** dans un conteneur **qui tourne déjà**.

&#x20;

```bash
# Exécute et rend la main
$ docker exec my-server2 whoami
root

# Ouvre un shell dedans
$ docker exec -it my-server2 bash
root@3f72126a581a:/#
```

&#x20;

{% columns %}
{% column %}
**`docker run`**

<mark style="color:blue;">**Crée un nouveau**</mark> conteneur à partir d'une image.

🏗️ Construire une nouvelle maison.
{% endcolumn %}

{% column %}
**`docker exec`**

<mark style="color:blue;">**Entre dans**</mark> un conteneur qui tourne déjà.

🚪 Entrer dans une maison existante.
{% endcolumn %}
{% endcolumns %}

&#x20;

### Inspecter ce qui se passe

&#x20;

```bash
docker logs my-server         # les logs
docker logs -f my-server      # suivre les logs en direct
docker inspect my-server      # toute la configuration (JSON)
docker container stats        # CPU / RAM en temps réel
```

&#x20;

***

&#x20;

## <mark style="color:purple;">05</mark> · Publier un port

&#x20;

Par défaut, un conteneur est **isolé** : un serveur web qui écoute sur le port 80 **à l'intérieur** n'est pas joignable depuis ton navigateur. On **publie** le port avec `-p <port_hôte>:<port_conteneur>`.

&#x20;

```bash
docker run --name my-server2 -p 8080:80 nginx
```

&#x20;

```mermaid
flowchart LR
    B(["🌐 Navigateur<br/>localhost:8080"]) --> H
    subgraph HOST["💻 Machine hôte"]
        H["🔌 Port 8080"]
        subgraph DK["🐳 Docker"]
            subgraph CT["📦 Conteneur nginx"]
                P["🔌 Port 80"] --> W["🌐 Serveur web"]
            end
        end
        H -->|"-p 8080:80"| P
    end

    style B fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style HOST fill:#f8fafc,stroke:#64748b,color:#0f172a
    style DK fill:#fef3c7,stroke:#f59e0b,color:#78350f
    style CT fill:#dcfce7,stroke:#22c55e,color:#14532d
```

&#x20;

<figure><img src="https://placehold.co/1200x500/f8fafc/0f172a?text=Welcome+to+nginx!" alt="Page d'accueil nginx"><figcaption><p>Ce que tu obtiens en ouvrant http://localhost:8080.</p></figcaption></figure>

&#x20;

### 🏨 L'analogie de l'hôtel

&#x20;

L'hôtel (ta machine) a un **standard téléphonique**. La chambre 80 (le conteneur) a un téléphone, mais personne dehors ne peut l'appeler directement. Avec `-p 8080:80`, tu dis au standard : **« quand on appelle le poste 8080, transfère vers la chambre 80 »**.

&#x20;

{% hint style="warning" %}
**L'ordre est toujours `hôte:conteneur`.** Tu peux lancer trois nginx sur les ports `8080`, `8081`, `8082` de ta machine, tous pointant vers le port `80` de leur propre conteneur.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">06</mark> · En résumé

&#x20;

{% hint style="success" %}
* `docker run` = pull + create + start. Options clés : `-it`, `-d`, `--name`, `-p`, `-e`, `--rm`.
* `docker ps -a` liste tous les conteneurs, même arrêtés.
* Un conteneur arrêté **occupe encore de la place** → `docker rm` ou `docker container prune`.
* `docker exec -it <conteneur> bash` pour entrer dans un conteneur qui tourne.
* `-p hôte:conteneur` pour rendre un service accessible depuis ta machine.
{% endhint %}

&#x20;

<mark style="color:green;">**→ Suite :**</mark> [6. Les données et volumes](06-volumes.md)
