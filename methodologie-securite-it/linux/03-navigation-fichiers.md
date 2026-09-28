---
description: Se déplacer dans l'arborescence et créer, copier, déplacer ou supprimer des fichiers.
---

# 3. Naviguer et gérer les fichiers

## Anatomie d'une commande

Presque toutes les commandes suivent la même structure :

```
  ls   -l -a   /home/alice
  ─┬─  ──┬──   ─────┬─────
   │     │          └── argument(s) : sur quoi agir
   │     └── option(s) : comment agir (commencent par - ou --)
   └── commande : quoi faire
```

* Les options courtes peuvent se **combiner** : `ls -l -a` = `ls -la`.
* Les options longues s'écrivent avec deux tirets : `ls --all`.

## Survivre dans le terminal

Avant les commandes, quelques réflexes qui changent la vie :

| Raccourci / commande | Effet |
| --- | --- |
| <kbd>Tab</kbd> | **Autocomplétion** des commandes et des chemins. Appuie deux fois pour voir les choix possibles |
| <kbd>↑</kbd> / <kbd>↓</kbd> | Parcourir l'**historique** des commandes |
| <kbd>Ctrl</kbd> + <kbd>C</kbd> | **Interrompre** la commande en cours |
| <kbd>Ctrl</kbd> + <kbd>L</kbd> | **Effacer** l'écran (comme `clear`) |
| <kbd>Ctrl</kbd> + <kbd>R</kbd> | **Rechercher** dans l'historique |
| <kbd>Ctrl</kbd> + <kbd>D</kbd> | Quitter le shell (fin d'entrée) |
| `history` | Afficher l'historique |
| `man <commande>` | Le **manuel** complet d'une commande (<kbd>q</kbd> pour quitter) |
| `<commande> --help` | Aide rapide |

{% hint style="success" %}
**La touche Tab est ta meilleure amie.** Elle évite les fautes de frappe et te montre ce qui existe. Les pros ne tapent presque jamais un chemin en entier.
{% endhint %}

{% hint style="info" %}
**Perdu devant une commande ?** `man ls` affiche son mode d'emploi. Dans `man`, tape `/mot` pour chercher un mot, <kbd>n</kbd> pour l'occurrence suivante, <kbd>q</kbd> pour quitter.
{% endhint %}

## Où suis-je ? `pwd`

`pwd` (_print working directory_) affiche le **dossier courant**.

```
$ pwd
/home/alice
```

### 🗺️ L'analogie du plan du centre commercial

`pwd`, c'est le point rouge **« Vous êtes ici »** sur le plan du centre commercial.

## Qu'y a-t-il ici ? `ls`

`ls` (_list_) affiche le contenu d'un dossier.

```bash
ls              # contenu du dossier courant
ls /etc         # contenu d'un autre dossier
ls -l           # format long (détails)
ls -a           # affiche aussi les fichiers cachés (qui commencent par .)
ls -la          # les deux
ls -lh          # tailles lisibles (K, M, G)
ls -R           # récursif : tous les sous-dossiers
ls -lt          # trié par date de modification
```

### Lire la sortie de `ls -l`

```
$ ls -l
-rw-r--r-- 1 alice staff  1.2K Sep 14 10:32 notes.txt
drwxr-xr-x 2 alice staff  4.0K Sep 14 09:15 Documents
──┬─────── ┬ ──┬── ──┬──  ──┬─ ──────┬───── ────┬────
  │        │   │     │      │        │          └── nom
  │        │   │     │      │        └── date de dernière modification
  │        │   │     │      └── taille
  │        │   │     └── groupe propriétaire
  │        │   └── utilisateur propriétaire
  │        └── nombre de liens
  └── type (- fichier, d dossier, l lien) + permissions (voir chapitre 7)
```

{% hint style="info" %}
Les **fichiers cachés** commencent par un point : `.bashrc`, `.zshrc`, `.gitignore`… Ce sont souvent des fichiers de configuration. `ls -a` les montre.
{% endhint %}

## Se déplacer : `cd`

`cd` (_change directory_) change le dossier courant.

```bash
cd /etc              # chemin absolu
cd Documents         # chemin relatif
cd ..                # remonter d'un niveau
cd ../..             # remonter de deux niveaux
cd ~                 # aller dans son home
cd                   # idem
cd -                 # revenir au dossier précédent
```

## Créer

```bash
mkdir projets                 # créer un dossier
mkdir -p a/b/c                # créer toute une hiérarchie d'un coup
touch notes.txt               # créer un fichier vide (ou mettre à jour sa date)
```

## Copier, déplacer, renommer

```bash
cp notes.txt copie.txt        # copier un fichier
cp notes.txt Documents/       # copier dans un dossier
cp -r projets/ sauvegarde/    # copier un DOSSIER (récursif, -r obligatoire)

mv notes.txt Documents/       # déplacer
mv ancien.txt nouveau.txt     # renommer (déplacer au même endroit sous un autre nom)
```

{% hint style="info" %}
Il n'y a pas de commande « renommer » sous Linux : **renommer, c'est déplacer** vers un nouveau nom, avec `mv`.
{% endhint %}

## Supprimer

```bash
rm notes.txt                  # supprimer un fichier
rm -i notes.txt               # demande confirmation avant
rmdir vide/                   # supprimer un dossier VIDE
rm -r projets/                # supprimer un dossier et tout son contenu
```

{% hint style="danger" %}
**Il n'y a pas de corbeille dans le terminal.** Un fichier supprimé avec `rm` est **définitivement perdu**.

Méfie-toi particulièrement de `rm -rf` (`-r` récursif, `-f` sans confirmation). Une faute de frappe comme `rm -rf / home/alice/tmp` (avec un espace en trop) tente d'effacer **tout le système**. Relis toujours deux fois avant d'appuyer sur Entrée.
{% endhint %}

## Les jokers (wildcards)

Le shell peut désigner **plusieurs fichiers d'un coup** grâce à des caractères spéciaux :

| Joker | Signification | Exemple | Correspond à |
| --- | --- | --- | --- |
| `*` | N'importe quelle suite de caractères | `*.txt` | `notes.txt`, `a.txt`, `todo.txt` |
| `?` | Un seul caractère | `photo?.jpg` | `photo1.jpg`, `photoA.jpg` |
| `[...]` | Un caractère parmi une liste | `rapport[12].pdf` | `rapport1.pdf`, `rapport2.pdf` |

```bash
ls *.txt                # tous les fichiers .txt
cp *.jpg Photos/        # copier toutes les images jpg
rm brouillon*           # supprimer tout ce qui commence par "brouillon"
```

{% hint style="warning" %}
Avant un `rm` avec un joker, fais d'abord un `ls` avec le **même** motif pour vérifier ce qui sera supprimé.
{% endhint %}

## Trouver des fichiers : `find`

```bash
find . -name "*.txt"            # tous les .txt sous le dossier courant
find /home -name "notes*"       # par nom, à partir de /home
find . -type d                  # uniquement les dossiers
find . -size +100M              # fichiers de plus de 100 Mo
```

## Qui suis-je ? Où est ce programme ?

```bash
whoami           # ton nom d'utilisateur
which ls         # où se trouve le programme ls → /usr/bin/ls
file notes.txt   # quel type de fichier ?
du -sh Documents # taille d'un dossier
df -h            # espace libre sur les disques
```

## En résumé

| Besoin | Commande |
| --- | --- |
| Où suis-je ? | `pwd` |
| Qu'y a-t-il ici ? | `ls -la` |
| Aller ailleurs | `cd <chemin>` |
| Créer un dossier / fichier | `mkdir` / `touch` |
| Copier / déplacer / renommer | `cp` / `mv` |
| Supprimer | `rm` (dossier : `rm -r`) ⚠️ |
| Trouver un fichier | `find` |
| Obtenir de l'aide | `man <commande>` / `--help` |
