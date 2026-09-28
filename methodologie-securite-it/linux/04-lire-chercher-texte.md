---
description: Afficher, parcourir, filtrer et compter du texte avec cat, less, head, tail, echo, grep, wc et sort.
---

# 4. Lire et chercher du texte

Sous Linux, énormément de choses sont du **texte** : fichiers de configuration, logs, code, sorties de commandes. Savoir le lire et le filtrer est une compétence clé.

Pour les exemples, imagine ce fichier `fruits.txt` :

```
pomme
banane
cerise
pomme
kiwi
Banane
abricot
```

## Afficher du texte : `echo`

`echo` affiche simplement ce qu'on lui donne.

```bash
echo "Bonjour le monde"       # → Bonjour le monde
echo $HOME                    # → /home/alice (valeur d'une variable)
echo "Je suis $USER"          # → Je suis alice
echo "ligne" > fichier.txt    # écrire dans un fichier (voir chapitre 5)
```

{% hint style="info" %}
**Guillemets doubles ou simples ?**

* `"..."` : les variables sont **remplacées** → `echo "$HOME"` affiche `/home/alice`
* `'...'` : tout est pris **littéralement** → `echo '$HOME'` affiche `$HOME`
{% endhint %}

## Afficher un fichier : `cat`

`cat` (_concatenate_) affiche **tout le contenu** d'un fichier d'un coup.

```bash
cat fruits.txt                # affiche le fichier
cat -n fruits.txt             # avec les numéros de ligne
cat a.txt b.txt               # affiche les deux à la suite (concatène)
cat a.txt b.txt > ab.txt      # fusionne deux fichiers en un
```

{% hint style="warning" %}
`cat` est parfait pour les **petits** fichiers. Sur un fichier de 10 000 lignes, tout défile et tu ne vois que la fin. Utilise plutôt `less`.
{% endhint %}

## Parcourir un gros fichier : `less`

`less` ouvre un fichier **page par page**, dans lequel tu peux naviguer et chercher.

```bash
less /var/log/syslog
```

| Touche | Action |
| --- | --- |
| <kbd>Espace</kbd> / <kbd>b</kbd> | Page suivante / précédente |
| <kbd>↑</kbd> / <kbd>↓</kbd> | Ligne par ligne |
| <kbd>g</kbd> / <kbd>G</kbd> | Début / fin du fichier |
| `/mot` | **Chercher** « mot » vers le bas |
| <kbd>n</kbd> / <kbd>N</kbd> | Occurrence suivante / précédente |
| <kbd>q</kbd> | **Quitter** |

### 📖 L'analogie du livre

* `cat`, c'est **jeter toutes les pages du livre sur la table** d'un coup.
* `less`, c'est **lire le livre page par page**, avec un index pour chercher.

{% hint style="info" %}
_« less is more »_ : `less` est une version améliorée d'un ancien outil nommé `more`, qui ne permettait que d'avancer. D'où le jeu de mots.
{% endhint %}

## Le début ou la fin : `head` et `tail`

```bash
head fruits.txt               # les 10 premières lignes
head -n 3 fruits.txt          # les 3 premières lignes
tail fruits.txt               # les 10 dernières lignes
tail -n 2 fruits.txt          # les 2 dernières lignes
tail -f /var/log/syslog       # suit le fichier EN DIRECT (Ctrl+C pour arrêter)
```

{% hint style="success" %}
`tail -f` est l'outil n°1 pour **surveiller des logs** en temps réel : chaque nouvelle ligne écrite dans le fichier s'affiche immédiatement.
{% endhint %}

## Chercher : `grep`

`grep` affiche **uniquement les lignes qui contiennent un motif**. C'est l'un des outils les plus utilisés sous Linux.

```
$ grep pomme fruits.txt
pomme
pomme
```

### Les options essentielles

| Option | Effet | Exemple |
| --- | --- | --- |
| `-i` | Ignore la casse (majuscules / minuscules) | `grep -i banane` → `banane` et `Banane` |
| `-n` | Affiche le numéro de ligne | `grep -n kiwi` → `5:kiwi` |
| `-v` | **Inverse** : lignes qui ne contiennent **pas** le motif | `grep -v pomme` |
| `-c` | **Compte** les lignes trouvées | `grep -c pomme` → `2` |
| `-r` | Cherche **récursivement** dans tout un dossier | `grep -r "TODO" src/` |
| `-l` | Affiche seulement le **nom des fichiers** qui contiennent le motif | `grep -rl "password" .` |
| `-w` | Mot entier uniquement | `grep -w an` ne trouve pas « banane » |

```bash
grep -i "error" /var/log/syslog         # toutes les erreurs, peu importe la casse
grep -rn "TODO" .                       # tous les TODO du projet, avec fichier et ligne
grep -v "^#" /etc/ssh/sshd_config       # la config sans les lignes de commentaire
```

### Un aperçu des expressions régulières

`grep` comprend des motifs appelés **expressions régulières** (_regex_) :

| Motif | Signification | Exemple |
| --- | --- | --- |
| `^` | Début de ligne | `grep "^b"` → lignes qui commencent par b |
| `$` | Fin de ligne | `grep "e$"` → lignes qui finissent par e |
| `.` | N'importe quel caractère | `grep "k.w"` → `kiwi` |
| `*` | Le caractère précédent, 0 fois ou plus | `grep "po*"` |

### 🔦 L'analogie du surligneur

`grep`, c'est passer un **surligneur** sur un document et **ne garder que les lignes surlignées**.

## Compter : `wc`

`wc` (_word count_) compte les lignes, mots et caractères.

```bash
wc fruits.txt            # →  7  7  49 fruits.txt   (lignes, mots, octets)
wc -l fruits.txt         # → 7  (nombre de lignes uniquement)
wc -w fruits.txt         # nombre de mots
```

## Trier et dédoublonner : `sort` et `uniq`

```bash
sort fruits.txt          # tri alphabétique
sort -r fruits.txt       # tri inversé
sort -n nombres.txt      # tri numérique (sinon 10 arrive avant 9)
sort fruits.txt | uniq   # supprime les doublons
sort fruits.txt | uniq -c   # compte les occurrences de chaque ligne
```

{% hint style="warning" %}
`uniq` ne supprime que les doublons **consécutifs**. Il faut donc presque toujours **trier avant** : `sort | uniq`.
{% endhint %}

## Comparer deux fichiers : `diff`

```bash
diff ancien.txt nouveau.txt    # affiche les lignes qui diffèrent
```

## En résumé

| Besoin | Commande |
| --- | --- |
| Afficher un message ou une variable | `echo` |
| Afficher un petit fichier | `cat` |
| Parcourir un gros fichier | `less` (quitter avec <kbd>q</kbd>) |
| Le début / la fin | `head` / `tail` (`-f` pour suivre) |
| Chercher des lignes | `grep` (`-i`, `-n`, `-v`, `-r`) |
| Compter | `wc -l` |
| Trier / dédoublonner | `sort` / `sort \| uniq` |

La vraie puissance arrive quand on **combine** ces outils avec des pipes : c'est le [chapitre suivant](05-redirections-pipes.md).
