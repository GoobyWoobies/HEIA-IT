---
description: Questions pour vérifier ta compréhension de Linux et du shell. Clique pour voir la réponse.
---

# 10. Questions de révision

Essaie de répondre **avant** d'ouvrir la réponse. 💪

## Linux et le shell

<details>

<summary>1. Pourquoi la FSF parle-t-elle de « GNU/Linux » plutôt que de « Linux » ?</summary>

Parce que **Linux** n'est que le **noyau** (écrit par Linus Torvalds en 1991). Le reste du système (compilateur, shell, commandes de base…) vient du **projet GNU** lancé par Richard Stallman. Le système complet est donc la combinaison des deux.

</details>

<details>

<summary>2. Qu'est-ce qu'un shell ? Et quelle différence avec un terminal ?</summary>

Le **shell** est un **interpréteur de commandes** : il lit ce que tu tapes et demande au système de l'exécuter (ex. bash, zsh).
Le **terminal** est la **fenêtre** qui affiche le texte et capte le clavier. Le shell tourne **dans** le terminal.

</details>

<details>

<summary>3. Que se passe-t-il quand je tape <code>ls</code> ?</summary>

1. Le **shell** interprète la commande et lance le programme `ls`.
2. `ls` demande le contenu du dossier au **noyau** via un **appel système**.
3. Le noyau lit l'information sur le **matériel** (disque).
4. Le résultat remonte jusqu'à `ls`, qui l'affiche.

</details>

<details>

<summary>4. Quel est le shell par défaut de la plupart des distributions Linux ? Et de macOS ?</summary>

**bash** pour la plupart des Linux, **zsh** pour macOS (depuis 2019).

</details>

## Arborescence

<details>

<summary>5. Dans quel dossier trouve-t-on : (a) les fichiers de configuration, (b) les logs, (c) les dossiers personnels, (d) les fichiers temporaires ?</summary>

* (a) `/etc`
* (b) `/var/log`
* (c) `/home`
* (d) `/tmp`

</details>

<details>

<summary>6. Que représentent <code>~</code>, <code>.</code> et <code>..</code> ?</summary>

* `~` : ton dossier personnel (`$HOME`)
* `.` : le dossier courant
* `..` : le dossier parent

</details>

<details>

<summary>7. Je suis dans <code>/home/alice</code>. Donne le chemin absolu et relatif vers <code>/home/bob/todo.txt</code>.</summary>

* Absolu : `/home/bob/todo.txt`
* Relatif : `../bob/todo.txt`

</details>

<details>

<summary>8. Que signifie « tout est fichier » sous Linux ? Donne un exemple.</summary>

Les documents, mais aussi les dossiers, les **périphériques** et les informations système sont représentés comme des fichiers. Exemples : `/dev/sda` (un disque), `/proc/cpuinfo` (infos sur le processeur, lisible avec `cat`).

</details>

## Commandes

<details>

<summary>9. Comment copier un dossier entier avec son contenu ?</summary>

`cp -r source/ destination/` — l'option `-r` (récursif) est obligatoire pour un dossier.

</details>

<details>

<summary>10. Comment renommer un fichier sous Linux ?</summary>

Avec `mv` : `mv ancien.txt nouveau.txt`. Renommer, c'est déplacer vers un nouveau nom.

</details>

<details>

<summary>11. Pourquoi faut-il être très prudent avec <code>rm -rf</code> ?</summary>

Parce qu'il supprime **récursivement** et **sans confirmation**, et qu'il n'y a **pas de corbeille** : tout est perdu définitivement. Une faute de frappe peut effacer tout le système.

</details>

<details>

<summary>12. Quand utiliser <code>less</code> plutôt que <code>cat</code> ?</summary>

Pour les **gros fichiers**. `cat` affiche tout d'un coup (on ne voit que la fin), alors que `less` permet de naviguer page par page et de chercher avec `/`.

</details>

<details>

<summary>13. Quelle commande pour suivre un fichier de log en temps réel ?</summary>

`tail -f <fichier>`

</details>

<details>

<summary>14. Comment afficher toutes les lignes d'un fichier qui contiennent « error », sans tenir compte des majuscules, avec les numéros de ligne ?</summary>

`grep -in error <fichier>`

</details>

<details>

<summary>15. Pourquoi écrit-on <code>sort | uniq</code> et pas juste <code>uniq</code> ?</summary>

`uniq` ne supprime que les doublons **consécutifs**. Il faut d'abord trier pour que les lignes identiques soient côte à côte.

</details>

## Redirections et pipes

<details>

<summary>16. Quels sont les trois descripteurs de fichiers standard ?</summary>

| Nom | Numéro | Par défaut |
| --- | --- | --- |
| stdin | 0 | Clavier |
| stdout | 1 | Écran |
| stderr | 2 | Écran |

</details>

<details>

<summary>17. Quelle différence entre <code>></code> et <code>>></code> ?</summary>

* `>` **écrase** le fichier.
* `>>` **ajoute** à la fin du fichier.

</details>

<details>

<summary>18. Comment enregistrer uniquement les messages d'erreur d'une commande dans <code>err.txt</code> ?</summary>

`commande 2> err.txt`

</details>

<details>

<summary>19. Que fait <code>ls | sort -r | wc -l</code> ?</summary>

Liste les fichiers, les trie en ordre inverse, puis **compte le nombre de lignes**, c'est-à-dire le nombre de fichiers et dossiers. (Le tri ne change pas le résultat du comptage.)

</details>

<details>

<summary>20. Quelle est la différence entre une redirection et un pipe ?</summary>

Une **redirection** (`>`) envoie la sortie vers un **fichier**. Un **pipe** (`|`) envoie la sortie vers **une autre commande**.

</details>

## Variables d'environnement

<details>

<summary>21. À quoi sert la variable <code>PATH</code> ?</summary>

Elle contient la liste des dossiers (séparés par `:`) où le shell cherche les programmes, **dans l'ordre**, quand on tape une commande.

</details>

<details>

<summary>22. Comment ajouter <code>/opt/outils/bin</code> au PATH sans casser l'existant ?</summary>

`export PATH=$PATH:/opt/outils/bin`

Sans le `$PATH:` devant, on remplacerait toute la liste et plus aucune commande ne serait trouvée.

</details>

<details>

<summary>23. Comment rendre une variable permanente ?</summary>

L'ajouter dans le fichier de configuration du shell (`~/.zshrc` pour zsh, `~/.bashrc` pour bash), puis recharger avec `source ~/.zshrc`.

</details>

## Permissions

<details>

<summary>24. Que signifie <code>-rw-r--r--</code> ?</summary>

Un **fichier** (`-`) où :

* le propriétaire peut lire et écrire (`rw-`)
* le groupe peut seulement lire (`r--`)
* les autres peuvent seulement lire (`r--`)

En octal : **644**.

</details>

<details>

<summary>25. Convertis <code>rwxr-x---</code> en octal.</summary>

`rwx` = 4+2+1 = 7, `r-x` = 4+0+1 = 5, `---` = 0 → **750**.

</details>

<details>

<summary>26. Que signifie le droit <code>x</code> sur un dossier ?</summary>

Le droit d'**entrer** dans le dossier (`cd`) et d'accéder aux fichiers qu'il contient.

</details>

<details>

<summary>27. Mon script <code>./deploy.sh</code> affiche « Permission denied ». Que faire ?</summary>

Lui donner le droit d'exécution : `chmod u+x deploy.sh`.

</details>

<details>

<summary>28. Quelle différence entre <code>chown bob f.txt</code> et <code>chown bob:devs f.txt</code> ?</summary>

Le premier change **uniquement le propriétaire**. Le second change le propriétaire **et le groupe**.

</details>

<details>

<summary>29. Qu'est-ce que le principe du moindre privilège ? Donne deux exemples d'application.</summary>

Donner à chaque utilisateur **uniquement les accès nécessaires** à son travail, rien de plus.

Exemples : ne pas faire tourner une app en `root` dans un conteneur Docker ; donner un token d'API en lecture seule si l'écriture n'est pas utile ; un utilisateur de base de données sans droits d'administration.

</details>

<details>

<summary>30. Pourquoi <code>chmod 777</code> est-il une mauvaise idée ?</summary>

Il donne **tous les droits à tout le monde** : n'importe quel utilisateur ou processus peut lire, modifier et exécuter le fichier. On « répare » un problème de droits en ouvrant une faille de sécurité.

</details>

## Systèmes de fichiers

<details>

<summary>31. Quel est l'avantage d'un système de fichiers journalisé comme ext4 ?</summary>

Il note les modifications dans un journal **avant** de les appliquer. En cas de coupure brutale, il peut récupérer un état cohérent au lieu de laisser des fichiers corrompus.

</details>

<details>

<summary>32. Que contient <code>/proc</code> ? Prend-il de la place sur le disque ?</summary>

Des informations sur les **processus** et le **noyau**, générées à la volée par le noyau (procfs). Il ne prend **aucune place sur le disque** : c'est un système de fichiers virtuel.

</details>

<details>

<summary>33. Quelle est la particularité de tmpfs ?</summary>

Les fichiers sont stockés **en RAM** : très rapide, mais **tout est effacé au redémarrage**.

</details>
