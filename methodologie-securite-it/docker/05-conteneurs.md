---
description: Lancer, arrêter, supprimer et interagir avec des conteneurs, et publier leurs ports.
---

# 5. Les conteneurs

Un conteneur est **une image qui tourne**. Cette page couvre tout ce qu'il faut pour les manipuler au quotidien.

## Lancer un conteneur : `docker run`

```bash
docker run ohmyzsh/zsh
```

`docker run` fait en réalité **trois choses** d'un coup :

1. **pull** l'image si elle n'est pas présente localement ;
2. **crée** le conteneur ;
3. le **démarre** en exécutant sa commande par défaut (`CMD` / `ENTRYPOINT`).

{% hint style="info" %}
Un conteneur vit **aussi longtemps que son processus principal**. Si la commande se termine (un script qui s'arrête, un shell sans terminal attaché…), le conteneur s'arrête aussi. C'est pour ça que `docker run ohmyzsh/zsh` se termine immédiatement : sans terminal, le shell n'a rien à faire et quitte.
{% endhint %}

### Les options les plus utiles

| Option | Effet | Exemple |
| --- | --- | --- |
| `-it` | Mode **interactif** : attache ton clavier (stdin) et ton écran (stdout) au conteneur | `docker run -it ubuntu bash` |
| `--name` | Donne un **nom** au conteneur (sinon Docker en invente un, ex. `condescending_tharp`) | `--name my-server` |
| `-d` | Mode **détaché** : le conteneur tourne en arrière-plan | `docker run -d nginx` |
| `-p hôte:conteneur` | **Publie un port** (voir plus bas) | `-p 8080:80` |
| `-e` | Définit une **variable d'environnement** | `-e POSTGRES_PASSWORD=secret` |
| `-v` / `--mount` | Monte un **volume** (voir [chapitre 6](06-volumes.md)) | `-v data:/var/lib/data` |
| `--rm` | **Supprime automatiquement** le conteneur quand il s'arrête | `docker run --rm -it alpine sh` |

```bash
# Un shell interactif dans un conteneur nommé
docker run -it --name my-zsh ohmyzsh/zsh

# Un serveur web en arrière-plan
docker run -d --name my-server -p 8080:80 nginx
```

{% hint style="success" %}
**Astuce :** `docker run --rm -it <image> sh` est parfait pour tester rapidement quelque chose dans un environnement jetable.
{% endhint %}

## Lister les conteneurs

```bash
docker container ls      # ou : docker ps       → conteneurs EN COURS
docker container ls -a   # ou : docker ps -a    → TOUS, y compris arrêtés
```

```
$ docker container ls -a
CONTAINER ID   IMAGE   COMMAND                  CREATED          STATUS                     PORTS    NAMES
3f72126a581a   nginx   "/docker-entrypoint.…"   48 seconds ago   Up 48 seconds              80/tcp   my-server2
6fd0cc9552ea   nginx   "/docker-entrypoint.…"   3 minutes ago    Exited (0) 2 minutes ago            my-server
```

## Arrêter et redémarrer

```bash
docker stop my-server        # par nom
docker stop 16aea41c1946     # ou par ID (les premiers caractères suffisent)
docker start my-server       # redémarre un conteneur arrêté
docker restart my-server
```

{% hint style="warning" %}
Un conteneur **arrêté n'est pas supprimé** ! Il existe toujours (visible avec `docker ps -a`) et **occupe de l'espace disque**. Pense à faire le ménage.
{% endhint %}

## Supprimer un conteneur

```bash
docker container rm my-server       # ou : docker rm my-server
docker container rm 16aea41c1946

docker container prune              # supprime TOUS les conteneurs arrêtés
```

## Exécuter une commande dans un conteneur qui tourne : `docker exec`

`docker exec` lance une commande **supplémentaire** dans un conteneur **déjà démarré**.

```bash
# Exécute la commande et rend la main
$ docker exec my-server2 whoami
root

# Ouvre un shell interactif dans le conteneur
$ docker exec -it my-server2 bash
root@3f72126a581a:/#
```

{% hint style="info" %}
**`run` vs `exec` :**

* `docker run` → **crée un nouveau** conteneur à partir d'une image.
* `docker exec` → **entre dans** un conteneur qui tourne déjà.

C'est la différence entre **construire une nouvelle maison** et **entrer dans une maison existante**.
{% endhint %}

Autres commandes pour inspecter un conteneur :

```bash
docker logs my-server         # affiche les logs
docker logs -f my-server      # suit les logs en direct (comme tail -f)
docker inspect my-server      # toute la configuration en JSON
docker container stats        # CPU / RAM utilisés en temps réel
```

## Publier un port

Par défaut, un conteneur est **isolé** : un serveur web qui écoute sur le port 80 **à l'intérieur** du conteneur n'est pas accessible depuis ton navigateur.

Pour y accéder, on **publie** le port avec `-p <port_hôte>:<port_conteneur>` :

```bash
docker run --name my-server2 -p 8080:80 nginx
```

```
                          MACHINE HÔTE
                  ┌──────────────────────────────────────┐
                  │                  DOCKER              │
                  │        ┌──────────────────────────┐  │
  Navigateur      │        │   CONTENEUR nginx        │  │
                  │        │                          │  │
  localhost:8080 ─┼─► 8080 ┼──────► 80  [serveur web] │  │
                  │   ▲    │         ▲                │  │
                  │   │    └─────────┼────────────────┘  │
                  └───┼──────────────┼───────────────────┘
                      │              │
               port de l'hôte   port du conteneur
                (publié avec -p 8080:80)
```

Il suffit ensuite d'ouvrir <http://localhost:8080> pour voir la page « Welcome to nginx! ».

### 🏨 L'analogie de l'hôtel

L'hôtel (ta machine) a un **standard téléphonique**. La chambre 80 (le conteneur) a un téléphone, mais personne dehors ne peut l'appeler directement. Avec `-p 8080:80`, tu dis au standard : **« quand quelqu'un appelle le poste 8080, transfère vers la chambre 80 »**.

{% hint style="warning" %}
L'ordre est toujours **`hôte:conteneur`**. `-p 8080:80` signifie « le port 8080 de ma machine redirige vers le port 80 du conteneur ». Tu peux lancer plusieurs nginx sur les ports `8080`, `8081`, `8082` de ta machine, tous pointant vers le port `80` de leur conteneur respectif.
{% endhint %}

## Le cycle de vie d'un conteneur

```
                               run
                                │
                                ▼
   create   ┌─────────┐ start ┌─────────┐  pause   ┌─────────┐
  ────────► │ CREATED │ ────► │ RUNNING │ ───────► │ PAUSED  │
            └────┬────┘       └──┬───▲──┘ ◄─────── └─────────┘
                 │               │   │    unpause
          remove │          stop │   │ start
                 │               ▼   │
                 │            ┌─────────┐
                 │            │ STOPPED │
                 │            └────┬────┘
                 ▼                 │ remove
            ┌─────────┐            │
            │ DELETED │ ◄──────────┘
            └─────────┘
```

| État | Description | Commande pour y arriver |
| --- | --- | --- |
| **Created** | Créé mais jamais démarré | `docker create` |
| **Running** | En cours d'exécution | `docker start` / `docker run` |
| **Paused** | Processus gelés en mémoire | `docker pause` |
| **Stopped** (Exited) | Arrêté, mais toujours présent sur le disque | `docker stop` |
| **Deleted** | Supprimé définitivement | `docker rm` |

## En résumé

* `docker run` = pull + create + start. Options clés : `-it`, `-d`, `--name`, `-p`, `-e`, `--rm`.
* `docker ps -a` liste tous les conteneurs, même arrêtés.
* Un conteneur arrêté **occupe encore de la place** → `docker rm` ou `docker container prune`.
* `docker exec -it <conteneur> bash` pour entrer dans un conteneur qui tourne.
* `-p hôte:conteneur` pour rendre un service accessible depuis ta machine.
