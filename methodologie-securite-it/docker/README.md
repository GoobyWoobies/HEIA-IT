---
description: Conteneuriser ses applications — de la théorie aux commandes du quotidien.
---

# 🐳 Docker

> _« Mais… ça marche sur ma machine ! »_
> Docker existe pour que cette phrase disparaisse.

## Ce que tu vas apprendre

À la fin de ce chapitre, tu sauras :

1. **Ce qu'est Docker** et en quoi un conteneur diffère d'une machine virtuelle
2. **Comment Docker fonctionne** : client, daemon, registry
3. **Ce qu'est une image**, comment elle est construite en couches et comment en créer une avec un `Dockerfile`
4. **Lancer et gérer des conteneurs** : démarrer, arrêter, supprimer, exécuter des commandes, publier des ports
5. **Gérer les données** avec les volumes, bind mounts et tmpfs
6. **Entretenir ton système** Docker (nettoyage, espace disque)
7. **Orchestrer plusieurs conteneurs** avec Docker Compose

## Plan du chapitre

| # | Page | Idée clé |
| --- | --- | --- |
| 1 | [Introduction](01-introduction.md) | Pourquoi Docker ? Conteneurs vs machines virtuelles |
| 2 | [Architecture](02-architecture.md) | Client, daemon, registry, images et conteneurs |
| 3 | [Les images](03-images.md) | Un modèle en lecture seule, construit en couches |
| 4 | [Le Dockerfile](04-dockerfile.md) | La recette pour fabriquer ses propres images |
| 5 | [Les conteneurs](05-conteneurs.md) | Lancer, arrêter, supprimer, publier des ports |
| 6 | [Les données et volumes](06-volumes.md) | Où stocker les données qui doivent survivre |
| 7 | [Maintenance](07-maintenance.md) | Faire le ménage et surveiller l'espace disque |
| 8 | [Docker Compose](08-docker-compose.md) | Lancer toute une application en une commande |
| 9 | [Aide-mémoire](09-aide-memoire.md) | Toutes les commandes importantes sur une page |
| 10 | [Questions de révision](10-questions-revision.md) | Vérifier que tout est compris |

{% hint style="info" %}
**Conseil :** lis les pages dans l'ordre, chaque notion s'appuie sur la précédente. Si tu as Docker installé, tape les commandes au fur et à mesure : c'est en pratiquant que ça rentre.
{% endhint %}

## Références

* [Docker — Get started](https://docs.docker.com/get-started/)
* [Docker Hub](https://hub.docker.com)
* [Building images](https://docs.docker.com/get-started/docker-concepts/building-images/)
* [Publishing ports](https://docs.docker.com/get-started/docker-concepts/running-containers/publishing-ports/)
* [Docker Compose](https://docs.docker.com/compose/)
* [Docker workshop](https://docs.docker.com/get-started/workshop/)

_Source : cours « IT Methodology — Docker », P. Rétornaz & A. Jungo, HEIA-FR._
