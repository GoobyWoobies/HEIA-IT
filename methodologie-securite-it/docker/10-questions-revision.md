---
description: Questions pour vérifier ta compréhension de Docker. Clique pour voir la réponse.
---

# 10. Questions de révision

Essaie de répondre **avant** d'ouvrir la réponse. 💪

## Concepts de base

<details>

<summary>1. Quelle est la différence principale entre un conteneur et une machine virtuelle ?</summary>

Une **VM** embarque un **système d'exploitation complet** (guest OS) au-dessus d'un hyperviseur : elle est lourde (plusieurs Go) et lente à démarrer.

Un **conteneur** **partage le noyau de l'OS hôte** et ne contient que l'application et ses dépendances : il est léger (quelques Mo) et démarre en quelques secondes.

</details>

<details>

<summary>2. Quels sont les trois composants de l'architecture Docker et leur rôle ?</summary>

* **Client** (`docker`) : l'interface en ligne de commande, qui envoie les ordres au daemon.
* **Daemon** (`dockerd`) : tourne sur l'hôte et fait le vrai travail (gère images, conteneurs, volumes, réseaux).
* **Registry** (ex. Docker Hub) : stocke et distribue les images.

</details>

<details>

<summary>3. Quelle est la différence entre une image et un conteneur ?</summary>

Une **image** est un **modèle en lecture seule** (comme une classe ou un moule).
Un **conteneur** est une **instance en cours d'exécution** de cette image (comme un objet ou un gâteau). On peut créer plusieurs conteneurs à partir d'une même image.

</details>

<details>

<summary>4. Docker est-il le seul outil pour gérer des conteneurs ?</summary>

Non. Il existe aussi **Podman** (sans daemon, rootless) et **containerd** (moteur bas niveau utilisé par Docker et Kubernetes). Tous suivent le standard **OCI**.

</details>

## Images et Dockerfile

<details>

<summary>5. Pourquoi les images sont-elles composées de couches ? Quel est l'avantage ?</summary>

Chaque instruction du Dockerfile crée une couche en lecture seule. Les couches sont **partagées** entre images et **mises en cache** : une couche commune n'est stockée et téléchargée qu'une fois, et seules les couches modifiées sont reconstruites.

</details>

<details>

<summary>6. Pourquoi faut-il copier <code>package.json</code> et lancer <code>npm install</code> AVANT de copier le reste du code ?</summary>

Pour **profiter du cache**. Docker reconstruit une couche modifiée **et toutes celles qui suivent**. Si le code est copié avant `npm install`, chaque modification du code relance l'installation des dépendances. En copiant d'abord uniquement les fichiers de dépendances, `npm install` reste en cache tant que les dépendances ne changent pas.

</details>

<details>

<summary>7. Quelle est la différence entre <code>RUN</code> et <code>CMD</code> ?</summary>

* `RUN` s'exécute **pendant le build** de l'image (ex. installer des paquets). Le résultat est figé dans l'image.
* `CMD` définit la commande exécutée **au démarrage de chaque conteneur**.

</details>

<details>

<summary>8. L'instruction <code>EXPOSE 80</code> rend-elle le port 80 accessible depuis ma machine ?</summary>

**Non.** `EXPOSE` ne fait que **documenter** le port utilisé par l'application. Pour le rendre accessible, il faut le **publier** au lancement du conteneur : `docker run -p 8080:80 <image>`.

</details>

<details>

<summary>9. À quoi sert un multi-stage build ?</summary>

À séparer l'étape de **compilation** (qui nécessite beaucoup d'outils) de l'étape d'**exécution**. L'image finale ne contient que le résultat compilé : elle est **plus petite** et **plus sûre** (moins de logiciels = moins de failles).

</details>

<details>

<summary>10. Cite trois raisons d'utiliser un fichier <code>.dockerignore</code>.</summary>

1. **Builds plus rapides** : moins de fichiers envoyés au daemon.
2. **Sécurité** : évite d'inclure des secrets (`.env`, clés) dans l'image.
3. **Images plus petites** : les fichiers inutiles ne sont pas copiés.

</details>

<details>

<summary>11. Pourquoi faut-il éviter le tag <code>latest</code> en production ?</summary>

`latest` est simplement le tag par défaut, il **ne garantit pas** la dernière version et peut changer à tout moment. Le build n'est donc pas reproductible. Il vaut mieux **fixer une version précise** (ex. `postgres:16.4`).

</details>

## Conteneurs

<details>

<summary>12. Que fait exactement <code>docker run</code> ?</summary>

1. **Télécharge** l'image si elle n'est pas présente localement.
2. **Crée** le conteneur.
3. **Démarre** le conteneur en exécutant sa commande par défaut.

</details>

<details>

<summary>13. Quelle est la différence entre <code>docker run</code> et <code>docker exec</code> ?</summary>

* `docker run` **crée un nouveau conteneur** à partir d'une image.
* `docker exec` **exécute une commande dans un conteneur qui tourne déjà**.

</details>

<details>

<summary>14. Dans <code>-p 8080:80</code>, quel port est celui de l'hôte ?</summary>

**8080**. L'ordre est toujours `hôte:conteneur`. Le port 8080 de ta machine est redirigé vers le port 80 du conteneur.

</details>

<details>

<summary>15. J'ai arrêté un conteneur avec <code>docker stop</code>. Occupe-t-il encore de la place ?</summary>

**Oui.** Un conteneur arrêté existe toujours (visible avec `docker ps -a`) avec sa couche inscriptible. Il faut le supprimer avec `docker rm` ou `docker container prune`.

</details>

<details>

<summary>16. Cite les états du cycle de vie d'un conteneur.</summary>

**Created → Running ⇄ Paused**, **Running ⇄ Stopped**, puis **Deleted** (depuis Created ou Stopped).

</details>

## Données

<details>

<summary>17. Que deviennent les données écrites dans un conteneur quand on le supprime ?</summary>

Elles sont **perdues**, car elles se trouvent dans la couche inscriptible du conteneur, supprimée avec lui. Pour les conserver, il faut utiliser un **volume** ou un **bind mount**.

</details>

<details>

<summary>18. Quelle différence entre un volume, un bind mount et un tmpfs ?</summary>

| Type | Persistant | Emplacement |
| --- | --- | --- |
| Volume | Oui | Zone gérée par Docker |
| Bind mount | Oui | Dossier choisi sur l'hôte |
| tmpfs | Non | RAM uniquement |

</details>

<details>

<summary>19. Quel type de stockage choisir pour : (a) une base de données, (b) développer en voyant ses modifications en direct, (c) un secret temporaire ?</summary>

* (a) **Volume**
* (b) **Bind mount**
* (c) **tmpfs**

</details>

## Maintenance et Compose

<details>

<summary>20. <code>docker system prune</code> supprime-t-il les volumes ?</summary>

**Non**, par défaut les volumes sont épargnés pour éviter la perte de données. Il faut ajouter `--volumes`.

</details>

<details>

<summary>21. En quoi le nettoyage régulier de Docker est-il une mesure de sécurité ?</summary>

De vieilles images oubliées peuvent contenir des **vulnérabilités connues**. Les supprimer **réduit la surface d'attaque** de la machine.

</details>

<details>

<summary>22. Quel problème Docker Compose résout-il ?</summary>

Il permet de **décrire une application multi-conteneurs** (frontend, backend, base de données…) dans **un seul fichier** versionné, et de tout lancer avec `docker compose up`. Plus besoin d'enchaîner de longues commandes `docker run` à la main.

</details>

<details>

<summary>23. Dans Compose, comment le backend peut-il joindre la base de données sans connaître son adresse IP ?</summary>

Compose crée un **réseau commun** où chaque service est joignable **par son nom**. Si le service s'appelle `db`, le backend s'y connecte simplement via l'hôte `db`.

</details>

<details>

<summary>24. Quelle est la différence entre <code>docker compose down</code> et <code>docker compose down -v</code> ?</summary>

* `down` arrête et supprime les **conteneurs et réseaux**.
* `down -v` supprime **en plus les volumes** → les données (ex. base de données) sont perdues.

</details>
