---
description: Faire le ménage dans Docker et surveiller l'espace disque utilisé.
---

# 7. Maintenance de base

Docker est très pratique… et très gourmand en disque si on ne fait jamais le ménage. Images anciennes, conteneurs arrêtés, cache de build : tout s'accumule **silencieusement**.

### 🧹 L'analogie du garage

Chaque `docker build`, `docker pull` ou `docker run` dépose un carton dans ton garage. Au bout de quelques mois, tu ne peux plus rentrer la voiture. **`docker system prune`**, c'est le grand rangement de printemps.

## Voir ce qui prend de la place

```
$ docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          123       8         9.926GB   8.019GB (80%)
Containers      14        1         257.1MB   257.1MB (100%)
Local Volumes   46        2         1.193GB   1.144GB (95%)
Build Cache     600       0         15.27GB   13.05GB
```

La colonne **RECLAIMABLE** indique l'espace qu'on peut récupérer. Ici, plus de **22 Go** pourraient être libérés !

## Tout nettoyer : `docker system prune`

```
$ docker system prune
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all dangling images
  - unused build cache

Are you sure you want to continue? [y/N]
```

Cette commande supprime :

* tous les **conteneurs arrêtés** ;
* tous les **réseaux inutilisés** ;
* toutes les **images « dangling »** (sans nom ni tag, typiquement d'anciennes versions remplacées par un nouveau build) ;
* le **cache de build inutilisé**.

### Pourquoi c'est important

* 💾 **Espace disque** : libère des Go de données inutiles et évite les erreurs « disque plein ».
* ⚡ **Performances** : un disque moins encombré, c'est un système plus réactif et des déploiements plus rapides.
* 🔒 **Sécurité** : de vieilles images oubliées peuvent contenir des **vulnérabilités connues**. Moins d'images = moins de surface d'attaque.
* 🌐 **Réseau propre** : supprime les configurations réseau (bridges, règles iptables) laissées par d'anciens conteneurs.
* 🤖 **Automatisable** : une seule commande à planifier, au lieu de scripts de nettoyage manuels.

## Et les volumes ?

{% hint style="danger" %}
Par défaut, `docker system prune` **ne supprime pas les volumes**, pour éviter de perdre des données importantes (ta base de données, par exemple).
{% endhint %}

Pour les inclure, il faut le demander explicitement :

```
$ docker system prune --volumes
WARNING! This will remove:
  - all stopped containers
  - all networks not used by at least one container
  - all anonymous volumes not used by at least one container
  - all dangling images
  - unused build cache
```

## Nettoyer de façon ciblée

| Commande | Ce qu'elle supprime |
| --- | --- |
| `docker container prune` | Conteneurs arrêtés |
| `docker image prune` | Images dangling (sans tag) |
| `docker image prune -a` | **Toutes** les images non utilisées par un conteneur |
| `docker volume prune` | Volumes non utilisés |
| `docker network prune` | Réseaux non utilisés |
| `docker builder prune` | Cache de build |
| `docker system prune -a --volumes` | ⚠️ **Tout** ce qui n'est pas utilisé — le grand ménage |

{% hint style="warning" %}
Avant un `prune` avec `-a` ou `--volumes`, **vérifie** avec `docker system df` et `docker volume ls` que tu ne vas rien perdre d'important. Il n'y a **pas de corbeille**.
{% endhint %}

## Autres commandes `docker system`

```
$ docker system --help
Commands:
  df        Show docker disk usage
  events    Get real time events from the server
  info      Display system-wide information
  prune     Remove unused data
```

* `docker system info` → version, nombre de conteneurs et images, driver de stockage…
* `docker system events` → flux en temps réel de ce qui se passe (conteneur démarré, arrêté, image supprimée…)

## En résumé

* `docker system df` pour **mesurer**, `docker system prune` pour **nettoyer**.
* Les **volumes ne sont pas supprimés par défaut** : il faut `--volumes`.
* Nettoyer régulièrement, c'est aussi une **mesure de sécurité**.
