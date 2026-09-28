---
description: Conserver les données au-delà de la vie d'un conteneur avec les volumes, bind mounts et tmpfs.
---

# 6. Les données : volumes, bind mounts et tmpfs

## Le problème

On l'a vu : quand un conteneur est supprimé, **sa couche inscriptible disparaît avec lui**. Imagine une base de données PostgreSQL dans un conteneur : tu mets à jour l'image, tu recrées le conteneur… et **toutes tes données ont disparu**. 😱

Les conteneurs sont conçus pour être **jetables**. Les données, elles, ne doivent pas l'être. Il faut donc les stocker **en dehors** du conteneur.

## Les trois types de stockage

Les données dans Docker peuvent être **temporaires** ou **persistantes** :

```
  ┌──────────────────────────────────────────────────────────────────┐
  │ Machine hôte                                                     │
  │                                                                  │
  │   /home/moi/projet          ┌─────────── Docker ──────┐   ┌─────┐│
  │   ┌──────────┐              │                         │   │ RAM ││
  │   │ dossier  │◄─────────────┼── Conteneur 2           │   │┌───┐││
  │   │ de l'hôte│  bind mount  │                         │   ││tmp│││
  │   └──────────┘              │   Conteneur 1 ──────────┼──►││fs │││
  │                             │        │                │   │└───┘││
  │                             │        ▼                │   └─────┘│
  │                             │   ┌─────────┐           │          │
  │                             │   │ Volume  │ (géré     │          │
  │                             │   └─────────┘  par      │          │
  │                             │                Docker)  │          │
  │                             └─────────────────────────┘          │
  └──────────────────────────────────────────────────────────────────┘
```

| Type | Persistant ? | Où sont les données ? | Cas d'usage |
| --- | --- | --- | --- |
| **Volume** | ✅ Oui | Dans une zone gérée par Docker | Bases de données, données d'application |
| **Bind mount** | ✅ Oui | Dans un dossier **de ton choix** sur l'hôte | Développement : modifier le code sans reconstruire l'image |
| **tmpfs** | ❌ Non | En **RAM** uniquement | Données temporaires ou sensibles (jamais écrites sur disque) |

### 🎒 L'analogie de l'étudiant en location

Pense au conteneur comme à **une chambre d'étudiant meublée** que tu peux rendre à tout moment :

* **Volume** = un **casier de consigne** géré par la résidence. Tu y ranges tes affaires, et elles restent là même si tu changes de chambre. Tu ne sais pas exactement où il est, mais la résidence s'en occupe.
* **Bind mount** = un **carton qui vient de chez tes parents**. C'est ton dossier à toi, tu sais exactement où il est et tu peux le modifier depuis la maison.
* **tmpfs** = un **tableau blanc** dans la chambre. Pratique pour noter des trucs, mais tout est effacé quand tu pars.

## Les volumes

Un volume est un espace de stockage **créé et géré par Docker**.

### Gérer les volumes

```bash
docker volume create volume_test    # créer
docker volume ls                    # lister
docker volume inspect volume_test   # détails (dont l'emplacement réel)
docker volume rm volume_test        # supprimer
```

```
$ docker volume ls
DRIVER    VOLUME NAME
local     volume_test
```

### Monter un volume dans un conteneur

Deux syntaxes équivalentes :

{% tabs %}
{% tab title="--volume (courte)" %}
```bash
docker run -it --volume volume_test:/volume_test ohmyzsh/zsh
#                       └────┬────┘ └────┬─────┘
#                        volume      chemin dans
#                                    le conteneur
```
{% endtab %}

{% tab title="--mount (explicite)" %}
```bash
docker run -it \
  --mount type=volume,source=volume_test,target=/volume_test \
  ohmyzsh/zsh
```
{% endtab %}
{% endtabs %}

Tout ce qui est écrit dans `/volume_test` à l'intérieur du conteneur est stocké dans le volume et **survit à la suppression du conteneur**.

{% hint style="success" %}
Si le volume n'existe pas encore, Docker le **crée automatiquement** au lancement du conteneur.
{% endhint %}

### Exemple concret : PostgreSQL

```bash
docker run -d --name db \
  -e POSTGRES_PASSWORD=secret \
  -v pgdata:/var/lib/postgresql/data \
  postgres:16

docker rm -f db      # on supprime le conteneur…

docker run -d --name db \
  -e POSTGRES_PASSWORD=secret \
  -v pgdata:/var/lib/postgresql/data \
  postgres:16        # …les données sont toujours là ✅
```

### Pourquoi préférer les volumes ?

* Plus **faciles à sauvegarder et migrer** que les bind mounts.
* Gérables via la **CLI Docker** ou l'**API Docker**.
* Peuvent être **partagés plus sûrement** entre plusieurs conteneurs.
* Un nouveau volume peut être **pré-rempli** par le contenu du conteneur ou de l'image.
* Meilleures **performances d'entrée/sortie** (surtout sur macOS et Windows).

📖 [Documentation sur les volumes](https://docs.docker.com/engine/storage/volumes/)

## Les bind mounts

Un bind mount relie **un dossier précis de ta machine** à un dossier du conteneur. Les deux voient **exactement les mêmes fichiers**, en temps réel.

```bash
mkdir /app/test     # un dossier sur l'hôte

docker run -it --mount type=bind,source=/app/test,target=/mnt ohmyzsh/zsh
docker run -it --volume /app/test:/mnt ohmyzsh/zsh     # équivalent
```

```
   Machine hôte                     Conteneur
   ┌──────────────┐                ┌──────────────┐
   │  /app/test   │ ◄────────────► │     /mnt     │
   └──────────────┘   même contenu └──────────────┘
```

{% hint style="info" %}
**Comment Docker fait la différence avec `-v` ?** Si la partie gauche est un **chemin** (commence par `/` ou `./`), c'est un bind mount. Si c'est un **simple nom**, c'est un volume.

* `-v ./src:/app/src` → bind mount
* `-v mesdonnees:/data` → volume
{% endhint %}

**Cas d'usage typique :** en développement, tu montes ton code source dans le conteneur. Tu modifies un fichier dans ton éditeur, et le changement est **immédiatement visible** dans le conteneur, sans reconstruire l'image.

{% hint style="warning" %}
Un bind mount donne au conteneur un **accès direct à ton système de fichiers**. Ne monte jamais un dossier sensible (ex. `/` ou `/etc`) sans bonne raison.
{% endhint %}

## tmpfs

Un montage `tmpfs` stocke les données **uniquement en RAM**. Elles disparaissent dès que le conteneur s'arrête.

```bash
docker run -it --mount type=tmpfs,destination=/mnt --name mycon ohmyzsh/zsh
```

**Cas d'usage :** fichiers temporaires, cache, ou données sensibles (tokens, secrets) qu'on ne veut **jamais écrire sur le disque**.

{% hint style="info" %}
`tmpfs` n'est disponible que pour les conteneurs Linux.
{% endhint %}

## En résumé

| Besoin | Solution |
| --- | --- |
| Garder les données d'une base de données | **Volume** |
| Modifier mon code en direct pendant le développement | **Bind mount** |
| Données temporaires ou sensibles, jamais sur disque | **tmpfs** |

* Sans montage, **les données meurent avec le conteneur**.
* `-v nom:/chemin` → volume ; `-v /chemin/hote:/chemin` → bind mount.
