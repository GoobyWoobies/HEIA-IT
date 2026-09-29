---
description: 33 questions pour vérifier ta compréhension de Linux et du shell. Clique pour voir la réponse.
icon: graduation-cap
cover: https://placehold.co/1600x500/0f172a/4ade80?text=Linux+%C2%B7+R%C3%A9vision
coverY: 0
---

# 10. Questions de révision

<mark style="color:blue;">**Teste-toi avant l'examen.**</mark>

&#x20;

{% hint style="info" %}
**Mode d'emploi** — réponds **à voix haute ou par écrit** avant d'ouvrir la réponse. Si tu bloques, relis la page indiquée entre parenthèses.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Linux et le shell

&#x20;

<details>

<summary>1. Pourquoi la FSF parle-t-elle de « GNU/Linux » ? <em>(p. 1)</em></summary>

&#x20;

Parce que **Linux** n'est que le **noyau** (Linus Torvalds, 1991). Le reste du système (compilateur, shell, commandes…) vient du **projet GNU** de Richard Stallman.

&#x20;

</details>

<details>

<summary>2. Shell ou terminal : quelle différence ? <em>(p. 1)</em></summary>

&#x20;

Le **shell** interprète les commandes (bash, zsh). Le **terminal** est la **fenêtre** qui l'affiche. Le shell tourne **dans** le terminal.

&#x20;

</details>

<details>

<summary>3. Que se passe-t-il quand je tape <code>ls</code> ? <em>(p. 1)</em></summary>

&#x20;

1. Le **shell** lance le programme `ls`.
2. `ls` demande le contenu au **noyau** via un **appel système**.
3. Le noyau lit le **disque**.
4. Le résultat remonte à `ls`, qui l'affiche.

&#x20;

</details>

<details>

<summary>4. Shell par défaut de la plupart des Linux ? Et de macOS ? <em>(p. 1)</em></summary>

&#x20;

**bash** pour la plupart des Linux, **zsh** pour macOS.

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">02</mark> · Arborescence

&#x20;

<details>

<summary>5. Où trouve-t-on : (a) la config, (b) les logs, (c) les dossiers personnels, (d) le temporaire ? <em>(p. 2)</em></summary>

&#x20;

(a) `/etc` · (b) `/var/log` · (c) `/home` · (d) `/tmp`

&#x20;

</details>

<details>

<summary>6. Que représentent <code>~</code>, <code>.</code> et <code>..</code> ? <em>(p. 2)</em></summary>

&#x20;

`~` = ton home · `.` = le dossier courant · `..` = le dossier parent

&#x20;

</details>

<details>

<summary>7. Depuis <code>/home/alice</code>, chemins absolu et relatif vers <code>/home/bob/todo.txt</code> ? <em>(p. 2)</em></summary>

&#x20;

Absolu : `/home/bob/todo.txt` · Relatif : `../bob/todo.txt`

&#x20;

</details>

<details>

<summary>8. Que signifie « tout est fichier » ? <em>(p. 2)</em></summary>

&#x20;

Documents, dossiers, **périphériques** et **infos système** sont tous représentés comme des fichiers. Ex. : `/dev/sda` (un disque), `/proc/cpuinfo` (le processeur).

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">03</mark> · Commandes

&#x20;

<details>

<summary>9. Comment copier un dossier entier ? <em>(p. 3)</em></summary>

&#x20;

`cp -r source/ destination/` — `-r` est obligatoire pour un dossier.

&#x20;

</details>

<details>

<summary>10. Comment renommer un fichier ? <em>(p. 3)</em></summary>

&#x20;

`mv ancien.txt nouveau.txt` — renommer, c'est déplacer.

&#x20;

</details>

<details>

<summary>11. Pourquoi se méfier de <code>rm -rf</code> ? <em>(p. 3)</em></summary>

&#x20;

Suppression **récursive**, **sans confirmation**, **sans corbeille**. Une faute de frappe peut effacer tout le système.

&#x20;

</details>

<details>

<summary>12. Quand utiliser <code>less</code> plutôt que <code>cat</code> ? <em>(p. 4)</em></summary>

&#x20;

Pour les **gros fichiers** : `less` pagine et permet de chercher avec `/`.

&#x20;

</details>

<details>

<summary>13. Comment suivre un log en temps réel ? <em>(p. 4)</em></summary>

&#x20;

`tail -f <fichier>`

&#x20;

</details>

<details>

<summary>14. Afficher les lignes contenant « error », sans casse, avec numéros ? <em>(p. 4)</em></summary>

&#x20;

`grep -in error <fichier>`

&#x20;

</details>

<details>

<summary>15. Pourquoi <code>sort | uniq</code> et pas juste <code>uniq</code> ? <em>(p. 4)</em></summary>

&#x20;

`uniq` ne supprime que les doublons **consécutifs** : il faut trier d'abord.

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">04</mark> · Redirections et pipes

&#x20;

<details>

<summary>16. Les trois descripteurs standard ? <em>(p. 5)</em></summary>

&#x20;

| Nom    | FD | Par défaut |
| ------ | -- | ---------- |
| stdin  | 0  | Clavier    |
| stdout | 1  | Écran      |
| stderr | 2  | Écran      |

&#x20;

</details>

<details>

<summary>17. <code>></code> ou <code>>></code> ? <em>(p. 5)</em></summary>

&#x20;

`>` **écrase** le fichier, `>>` **ajoute** à la fin.

&#x20;

</details>

<details>

<summary>18. Enregistrer seulement les erreurs dans <code>err.txt</code> ? <em>(p. 5)</em></summary>

&#x20;

`commande 2> err.txt`

&#x20;

</details>

<details>

<summary>19. Que fait <code>ls | sort -r | wc -l</code> ? <em>(p. 5)</em></summary>

&#x20;

Liste, trie à l'envers, puis **compte les lignes** = le nombre de fichiers et dossiers. (Le tri ne change pas le compte.)

&#x20;

</details>

<details>

<summary>20. Redirection ou pipe : quelle différence ? <em>(p. 5)</em></summary>

&#x20;

Une **redirection** envoie vers un **fichier** ; un **pipe** envoie vers **une autre commande**.

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">05</mark> · Variables d'environnement

&#x20;

<details>

<summary>21. À quoi sert <code>PATH</code> ? <em>(p. 6)</em></summary>

&#x20;

C'est la liste ordonnée des dossiers où le shell cherche les programmes quand on tape une commande.

&#x20;

</details>

<details>

<summary>22. Ajouter <code>/opt/outils/bin</code> au PATH sans rien casser ? <em>(p. 6)</em></summary>

&#x20;

`export PATH=$PATH:/opt/outils/bin` — sans `$PATH:`, on remplacerait toute la liste.

&#x20;

</details>

<details>

<summary>23. Comment rendre une variable permanente ? <em>(p. 6)</em></summary>

&#x20;

L'ajouter dans `~/.zshrc` (zsh) ou `~/.bashrc` (bash), puis `source ~/.zshrc`.

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">06</mark> · Permissions

&#x20;

<details>

<summary>24. Que signifie <code>-rw-r--r--</code> ? <em>(p. 7)</em></summary>

&#x20;

Un **fichier** : le propriétaire lit et écrit, le groupe et les autres lisent seulement. En octal : **644**.

&#x20;

</details>

<details>

<summary>25. <code>rwxr-x---</code> en octal ? <em>(p. 7)</em></summary>

&#x20;

7 (4+2+1), 5 (4+0+1), 0 → **750**.

&#x20;

</details>

<details>

<summary>26. Que signifie <code>x</code> sur un dossier ? <em>(p. 7)</em></summary>

&#x20;

Le droit d'**entrer** dedans (`cd`) et d'accéder à ses fichiers.

&#x20;

</details>

<details>

<summary>27. <code>./deploy.sh</code> → « Permission denied ». Que faire ? <em>(p. 7)</em></summary>

&#x20;

`chmod u+x deploy.sh`

&#x20;

</details>

<details>

<summary>28. <code>chown bob f.txt</code> vs <code>chown bob:devs f.txt</code> ? <em>(p. 7)</em></summary>

&#x20;

Le premier change **seulement le propriétaire** ; le second change **propriétaire et groupe**.

&#x20;

</details>

<details>

<summary>29. Le principe du moindre privilège, avec deux exemples ? <em>(p. 7)</em></summary>

&#x20;

Donner **uniquement les accès nécessaires**. Ex. : pas de `root` dans un conteneur Docker ; un token d'API en lecture seule ; un utilisateur de BDD sans droits admin.

&#x20;

</details>

<details>

<summary>30. Pourquoi <code>chmod 777</code> est-il une mauvaise idée ? <em>(p. 7)</em></summary>

&#x20;

Il donne **tous les droits à tout le monde** : on « répare » un problème de droits en ouvrant une faille.

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">07</mark> · Systèmes de fichiers

&#x20;

<details>

<summary>31. L'avantage d'un système journalisé comme ext4 ? <em>(p. 8)</em></summary>

&#x20;

Il note les modifications **avant** de les faire : après une coupure, il revient à un état cohérent au lieu de laisser des fichiers corrompus.

&#x20;

</details>

<details>

<summary>32. Que contient <code>/proc</code> ? Prend-il de la place sur le disque ? <em>(p. 8)</em></summary>

&#x20;

Des infos sur les **processus** et le **noyau**, générées à la volée. **Aucune place sur le disque** : c'est virtuel.

&#x20;

</details>

<details>

<summary>33. La particularité de tmpfs ? <em>(p. 8)</em></summary>

&#x20;

Stocké **en RAM** : très rapide, mais **effacé au redémarrage**.

&#x20;

</details>

&#x20;

***

&#x20;

{% hint style="success" %}
**Tout juste ?** Bravo, tu es à l'aise dans le terminal. Prochaine étape : le chapitre [Docker](../docker/README.md), où tu vas utiliser tout ça dans des conteneurs.
{% endhint %}
