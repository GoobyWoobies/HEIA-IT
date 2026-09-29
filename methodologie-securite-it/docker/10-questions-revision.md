---
description: 24 questions pour vérifier ta compréhension de Docker. Clique pour voir la réponse.
icon: graduation-cap
cover: https://placehold.co/1600x500/0f172a/38bdf8?text=Docker+%C2%B7+R%C3%A9vision
coverY: 0
---

# 10. Questions de révision

<mark style="color:blue;">**Teste-toi avant l'examen.**</mark>

&#x20;

{% hint style="info" %}
**Mode d'emploi** — essaie de répondre **à voix haute ou par écrit** avant d'ouvrir la réponse. Si tu bloques, relis la page indiquée entre parenthèses.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Concepts de base

&#x20;

<details>

<summary>1. Quelle est la différence principale entre un conteneur et une machine virtuelle ? <em>(p. 1)</em></summary>

&#x20;

Une **VM** embarque un **système d'exploitation complet** au-dessus d'un hyperviseur : lourde (plusieurs Go), lente à démarrer.

Un **conteneur** **partage le noyau de l'OS hôte** et ne contient que l'application et ses dépendances : léger (quelques Mo), démarre en secondes.

&#x20;

</details>

<details>

<summary>2. Quels sont les trois composants de l'architecture Docker ? <em>(p. 2)</em></summary>

&#x20;

* **Client** (`docker`) : l'interface en ligne de commande, qui envoie les ordres.
* **Daemon** (`dockerd`) : tourne sur l'hôte et fait le vrai travail.
* **Registry** (ex. Docker Hub) : stocke et distribue les images.

&#x20;

</details>

<details>

<summary>3. Quelle est la différence entre une image et un conteneur ? <em>(p. 2)</em></summary>

&#x20;

L'**image** est un **modèle en lecture seule** (le moule, la classe). Le **conteneur** est une **instance qui tourne** (le gâteau, l'objet). Plusieurs conteneurs peuvent venir de la même image.

&#x20;

</details>

<details>

<summary>4. Docker est-il le seul outil pour gérer des conteneurs ? <em>(p. 1)</em></summary>

&#x20;

Non : **Podman** (sans daemon, rootless) et **containerd** (moteur bas niveau) aussi. Tous suivent le standard **OCI**.

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">02</mark> · Images et Dockerfile

&#x20;

<details>

<summary>5. Pourquoi les images sont-elles composées de couches ? <em>(p. 3)</em></summary>

&#x20;

Les couches sont **partagées** entre images et **mises en cache** : une couche commune n'est stockée et téléchargée qu'une fois, et seules les couches modifiées sont reconstruites.

&#x20;

</details>

<details>

<summary>6. Pourquoi copier <code>package.json</code> et lancer <code>npm install</code> AVANT le reste du code ? <em>(p. 4)</em></summary>

&#x20;

Pour **profiter du cache**. Docker reconstruit une couche modifiée **et toutes les suivantes**. Si le code était copié avant, chaque modification relancerait l'installation des dépendances.

&#x20;

</details>

<details>

<summary>7. Quelle est la différence entre <code>RUN</code> et <code>CMD</code> ? <em>(p. 4)</em></summary>

&#x20;

* `RUN` s'exécute **pendant le build** (ex. installer des paquets). Résultat figé dans l'image.
* `CMD` s'exécute **à chaque démarrage** d'un conteneur.

&#x20;

</details>

<details>

<summary>8. <code>EXPOSE 80</code> rend-il le port 80 accessible depuis ma machine ? <em>(p. 4)</em></summary>

&#x20;

**Non.** `EXPOSE` ne fait que **documenter**. Il faut **publier** au lancement : `docker run -p 8080:80 <image>`.

&#x20;

</details>

<details>

<summary>9. À quoi sert un multi-stage build ? <em>(p. 4)</em></summary>

&#x20;

À séparer la **compilation** (beaucoup d'outils) de l'**exécution**. L'image finale ne contient que le résultat : **plus petite** et **plus sûre**.

&#x20;

</details>

<details>

<summary>10. Trois raisons d'utiliser un <code>.dockerignore</code> ? <em>(p. 4)</em></summary>

&#x20;

1. **Builds plus rapides** — moins de fichiers envoyés au daemon.
2. **Sécurité** — pas de secrets (`.env`, clés) dans l'image.
3. **Images plus petites** — pas de fichiers inutiles copiés.

&#x20;

</details>

<details>

<summary>11. Pourquoi éviter le tag <code>latest</code> en production ? <em>(p. 3)</em></summary>

&#x20;

`latest` est juste le tag par défaut : il **ne garantit pas** la dernière version et peut changer à tout moment. Le build n'est pas reproductible. Mieux vaut **fixer une version** (ex. `postgres:16.4`).

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">03</mark> · Conteneurs

&#x20;

<details>

<summary>12. Que fait exactement <code>docker run</code> ? <em>(p. 5)</em></summary>

&#x20;

1. **Télécharge** l'image si elle manque.
2. **Crée** le conteneur.
3. **Démarre** le conteneur avec sa commande par défaut.

&#x20;

</details>

<details>

<summary>13. <code>docker run</code> vs <code>docker exec</code> ? <em>(p. 5)</em></summary>

&#x20;

* `docker run` **crée un nouveau conteneur**.
* `docker exec` **exécute une commande dans un conteneur qui tourne déjà**.

&#x20;

</details>

<details>

<summary>14. Dans <code>-p 8080:80</code>, quel port est celui de l'hôte ? <em>(p. 5)</em></summary>

&#x20;

**8080.** L'ordre est toujours `hôte:conteneur`.

&#x20;

</details>

<details>

<summary>15. Un conteneur arrêté occupe-t-il encore de la place ? <em>(p. 5)</em></summary>

&#x20;

**Oui.** Il existe toujours (`docker ps -a`) avec sa couche inscriptible. Il faut `docker rm` ou `docker container prune`.

&#x20;

</details>

<details>

<summary>16. Cite les états du cycle de vie d'un conteneur. <em>(p. 5)</em></summary>

&#x20;

**Created → Running ⇄ Paused**, **Running ⇄ Stopped**, puis **Deleted** (depuis Created ou Stopped).

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">04</mark> · Données

&#x20;

<details>

<summary>17. Que deviennent les données écrites dans un conteneur supprimé ? <em>(p. 6)</em></summary>

&#x20;

**Perdues** : elles étaient dans la couche inscriptible, supprimée avec le conteneur. Il faut un **volume** ou un **bind mount**.

&#x20;

</details>

<details>

<summary>18. Volume, bind mount, tmpfs : quelles différences ? <em>(p. 6)</em></summary>

&#x20;

| Type          | Persistant | Emplacement             |
| ------------- | ---------- | ----------------------- |
| Volume     | Oui        | Zone gérée par Docker   |
| Bind mount | Oui        | Dossier choisi sur l'hôte |
| tmpfs      | Non        | RAM uniquement          |

&#x20;

</details>

<details>

<summary>19. Quel stockage pour : (a) une base de données, (b) du code modifié en direct, (c) un secret temporaire ? <em>(p. 6)</em></summary>

&#x20;

(a) **Volume** · (b) **Bind mount** · (c) **tmpfs**

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">05</mark> · Maintenance et Compose

&#x20;

<details>

<summary>20. <code>docker system prune</code> supprime-t-il les volumes ? <em>(p. 7)</em></summary>

&#x20;

**Non**, pour éviter la perte de données. Il faut ajouter `--volumes`.

&#x20;

</details>

<details>

<summary>21. En quoi le nettoyage régulier est-il une mesure de sécurité ? <em>(p. 7)</em></summary>

&#x20;

De vieilles images oubliées contiennent des **vulnérabilités connues**. Les supprimer **réduit la surface d'attaque**.

&#x20;

</details>

<details>

<summary>22. Quel problème Docker Compose résout-il ? <em>(p. 8)</em></summary>

&#x20;

Il décrit une application **multi-conteneurs** dans **un seul fichier** versionné, et lance tout avec `docker compose up`, au lieu d'enchaîner des `docker run` à la main.

&#x20;

</details>

<details>

<summary>23. Comment le backend joint-il la base sans connaître son IP ? <em>(p. 8)</em></summary>

&#x20;

Compose crée un **réseau commun** où chaque service est joignable **par son nom**. Le backend se connecte simplement à l'hôte `db`.

&#x20;

</details>

<details>

<summary>24. <code>docker compose down</code> vs <code>docker compose down -v</code> ? <em>(p. 8)</em></summary>

&#x20;

* `down` supprime **conteneurs et réseaux**.
* `down -v` supprime **en plus les volumes** → les données sont perdues.

&#x20;

</details>

&#x20;

***

&#x20;

{% hint style="success" %}
**Tout juste ?** Bravo, tu maîtrises les bases de Docker. Prochaine étape : conteneuriser un de tes propres projets.
{% endhint %}
