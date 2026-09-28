---
description: Écrire un Dockerfile pour construire ses propres images, et les bonnes pratiques associées.
---

# 4. Le Dockerfile : construire ses images

Les images de Docker Hub sont pratiques, mais tôt ou tard tu voudras **empaqueter ta propre application**. Pour ça, on écrit un **Dockerfile**.

## Qu'est-ce qu'un Dockerfile ?

Un `Dockerfile` est un simple fichier texte (sans extension) qui contient **la recette pour construire une image**, étape par étape.

### 👨‍🍳 L'analogie de la recette

| Cuisine | Docker |
| --- | --- |
| La recette écrite | Le **Dockerfile** |
| Cuisiner en suivant la recette | `docker build` |
| Le plat surgelé prêt à réchauffer | L'**image** |
| Le plat réchauffé et servi | Le **conteneur** |

## Un premier exemple

Voici un Dockerfile pour une application Node.js :

```dockerfile
# 1. Partir d'une image de base qui contient déjà Node.js
FROM node:lts-alpine

# 2. Se placer dans le dossier /app à l'intérieur de l'image
WORKDIR /app

# 3. Copier la liste des dépendances et les installer
COPY package.json package-lock.json ./
RUN npm install

# 4. Copier le reste du code source
COPY . .

# 5. Documenter le port utilisé par l'application
EXPOSE 3000

# 6. Commande exécutée au démarrage du conteneur
CMD ["npm", "start"]
```

## Les instructions principales

| Instruction | Rôle | Exemple |
| --- | --- | --- |
| `FROM` | Image de base (toujours la première ligne) | `FROM python:3.12-slim` |
| `WORKDIR` | Dossier de travail pour les instructions suivantes | `WORKDIR /app` |
| `COPY` | Copie des fichiers de ta machine vers l'image | `COPY src/ ./src/` |
| `ADD` | Comme `COPY`, mais sait aussi extraire des archives et télécharger des URL | `ADD app.tar.gz /opt/` |
| `RUN` | Exécute une commande **pendant la construction** | `RUN apt-get install -y curl` |
| `ENV` | Définit une variable d'environnement | `ENV NODE_ENV=production` |
| `ARG` | Variable disponible uniquement pendant le build | `ARG VERSION=1.0` |
| `EXPOSE` | Documente le port utilisé par l'app | `EXPOSE 8080` |
| `CMD` | Commande par défaut **au lancement du conteneur** | `CMD ["python", "app.py"]` |
| `ENTRYPOINT` | Programme principal du conteneur (plus difficile à remplacer que `CMD`) | `ENTRYPOINT ["nginx"]` |

{% hint style="info" %}
**`RUN` vs `CMD` :** c'est la confusion la plus fréquente.

* `RUN` s'exécute **une fois, au moment du `docker build`** (ex. installer des paquets). Son résultat est figé dans l'image.
* `CMD` s'exécute **à chaque démarrage d'un conteneur** (ex. lancer le serveur).
{% endhint %}

## Construire l'image

```bash
docker build -t mon-app:1.0 .
```

* `-t mon-app:1.0` → donne un nom et un tag à l'image
* `.` → le **build context** : le dossier dont le contenu est envoyé au daemon (ici le dossier courant)

Puis on lance un conteneur à partir de cette image :

```bash
docker run -p 3000:3000 mon-app:1.0
```

## Le cache de build : l'ordre compte !

Chaque instruction `RUN`, `COPY` et `ADD` crée **une nouvelle couche**. Docker garde ces couches en **cache** : au build suivant, il ne reconstruit **que les couches qui ont changé… et toutes celles qui viennent après**.

```
  FROM node:lts-alpine           ✅ en cache
  WORKDIR /app                   ✅ en cache
  COPY package.json ./           ✅ en cache (package.json n'a pas changé)
  RUN npm install                ✅ en cache → gain de plusieurs minutes !
  COPY . .                       🔄 reconstruit (le code a changé)
  CMD ["npm", "start"]           🔄 reconstruit
```

{% hint style="success" %}
**Règle d'or :** place en haut ce qui **change rarement** (dépendances) et en bas ce qui **change souvent** (code source).
{% endhint %}

{% tabs %}
{% tab title="❌ Mauvais ordre" %}
```dockerfile
FROM node:lts-alpine
WORKDIR /app
COPY . .            # le code change à chaque commit…
RUN npm install     # …donc npm install est relancé à CHAQUE build 🐌
CMD ["npm", "start"]
```
{% endtab %}

{% tab title="✅ Bon ordre" %}
```dockerfile
FROM node:lts-alpine
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm install     # relancé seulement si les dépendances changent ⚡
COPY . .
CMD ["npm", "start"]
```
{% endtab %}
{% endtabs %}

## Avancé : le multi-stage build

Pour compiler une application, il faut souvent beaucoup d'outils (compilateur, SDK…) dont on n'a **plus besoin à l'exécution**. Le **multi-stage build** permet de séparer :

1. une étape de **build**, lourde, avec tous les outils ;
2. une étape de **runtime**, légère, qui ne récupère que le résultat compilé.

### 🏗️ L'analogie du chantier

On construit une maison avec des grues, des échafaudages et des bétonnières. Mais on ne **livre pas** la maison avec les grues dans le jardin ! On ne garde que la maison finie.

```dockerfile
# ─── Étape 1 : BUILD ─────────────────────────────────
FROM golang:1.26-alpine AS builder
WORKDIR /build
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o server ./cmd/server

# ─── Étape 2 : RUNTIME ───────────────────────────────
FROM alpine:3.20
RUN apk add --no-cache ca-certificates tzdata
WORKDIR /app
COPY --from=builder /build/server .     # on ne récupère QUE le binaire
EXPOSE 8080
CMD ["./server"]
```

* `AS builder` → donne un nom à une étape
* `COPY --from=builder` → copie un fichier depuis une étape précédente

Résultat : l'image finale fait quelques dizaines de Mo au lieu de plusieurs centaines, et contient moins de logiciels donc **moins de failles potentielles**.

## Le fichier `.dockerignore`

Quand tu lances `docker build .`, **tout le dossier** est envoyé au daemon. Le fichier `.dockerignore` (même syntaxe que `.gitignore`) permet d'exclure ce qui n'est pas nécessaire.

```
# .dockerignore
.git
.cache
node_modules
.env
*.log
```

Pourquoi c'est important :

* ⚡ **Builds plus rapides** : un gros `node_modules` ou l'historique `.git` n'est plus envoyé au daemon.
* 🔒 **Sécurité** : évite que des secrets (`.env`, clés privées, identifiants) se retrouvent dans l'image, où **n'importe qui ayant accès à l'image pourrait les extraire**.
* 📦 **Images plus petites** : un `COPY . .` ne copie plus les fichiers inutiles.

## `EXPOSE` ne publie pas de port !

{% hint style="warning" %}
**Différence importante :** l'instruction `EXPOSE` dans un Dockerfile **n'ouvre aucun port**. Elle sert uniquement de **documentation** : « cette application écoute sur ce port ».

Pour rendre un port accessible depuis ta machine, il faut le **publier au moment de lancer le conteneur** avec `-p` :

```bash
docker run -p 8080:80 nginx
```

**Le Dockerfile crée des images. Les ports publiés et les volumes s'appliquent aux conteneurs.**
{% endhint %}

## Bonnes pratiques

| # | Pratique | Pourquoi |
| --- | --- | --- |
| 1 | **Multi-stage builds** | L'image finale ne contient que ce dont l'app a besoin |
| 2 | **Image de base petite et fiable** | Utilise des images _Official_ ou _Verified Publisher_ (ex. `-alpine`, `-slim`) et n'installe pas de paquets inutiles |
| 3 | **Fixer les versions, reconstruire souvent** | `FROM node:22.9-alpine` plutôt que `node:latest` ; reconstruire avec `--pull` pour récupérer les correctifs de sécurité |
| 4 | **Exploiter le cache** | Étapes stables en haut, `.dockerignore` pour exclure le superflu |
| 5 | **Combiner `apt-get update && apt-get install`** | Dans un seul `RUN`, pour ne jamais réutiliser un index de paquets obsolète mis en cache |
| 6 | **Un rôle par conteneur** | Un conteneur = un service (web, base de données…), sans état, facile à remplacer et à dupliquer |

Exemple pour la pratique n°5 :

```dockerfile
# ✅ Un seul RUN : update et install toujours synchronisés
RUN apt-get update && apt-get install -y \
      curl \
      git \
    && rm -rf /var/lib/apt/lists/*
```

📖 Référence : [Dockerfile best practices](https://docs.docker.com/build/building/best-practices/) · [Référence Dockerfile](https://docs.docker.com/reference/dockerfile/)

## En résumé

* Un **Dockerfile** est la recette d'une image ; `docker build -t nom:tag .` la construit.
* `RUN` s'exécute au build, `CMD` au démarrage du conteneur.
* **L'ordre des instructions compte** pour profiter du cache.
* Le **multi-stage build** produit des images légères et plus sûres.
* `.dockerignore` accélère les builds et protège tes secrets.
* `EXPOSE` documente, `-p` publie.
