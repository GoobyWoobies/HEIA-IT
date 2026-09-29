---
description: Se déplacer dans l'arborescence et créer, copier, déplacer ou supprimer des fichiers.
icon: compass
cover: https://placehold.co/1600x500/0f172a/4ade80?text=Linux+%C2%B7+Navigation
coverY: 0
---

# 3. Naviguer et gérer les fichiers

<mark style="color:blue;">**Où suis-je, qu'y a-t-il ici, et comment je range tout ça.**</mark>

&#x20;

{% hint style="info" %}
**En bref**

`pwd` te dit où tu es, `ls` ce qu'il y a, `cd` t'emmène ailleurs. Ensuite : `mkdir`, `touch`, `cp`, `mv`, `rm` pour créer, copier, déplacer et supprimer.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Anatomie d'une commande

&#x20;

```mermaid
flowchart LR
    C["ls<br/><i>commande</i><br/>quoi faire"] --- O["-l -a<br/><i>options</i><br/>comment le faire"] --- A["/home/alice<br/><i>argument</i><br/>sur quoi"]

    style C fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style O fill:#ede9fe,stroke:#8b5cf6,color:#4c1d95
    style A fill:#dcfce7,stroke:#22c55e,color:#14532d
```

&#x20;

* Les options courtes se **combinent** : `ls -l -a` = `ls -la`
* Les options longues ont deux tirets : `ls --all`

&#x20;

***

&#x20;

## <mark style="color:purple;">02</mark> · Les réflexes de survie

&#x20;

| Raccourci                           | Effet                                                              |
| ----------------------------------- | ------------------------------------------------------------------ |
| <kbd>Tab</kbd>                      | <mark style="color:green;">**Autocomplétion**</mark> — deux fois pour voir les choix |
| <kbd>↑</kbd> / <kbd>↓</kbd>         | Parcourir l'**historique**                                          |
| <kbd>Ctrl</kbd> + <kbd>C</kbd>      | **Interrompre** la commande en cours                                |
| <kbd>Ctrl</kbd> + <kbd>L</kbd>      | **Effacer** l'écran                                                 |
| <kbd>Ctrl</kbd> + <kbd>R</kbd>      | **Rechercher** dans l'historique                                    |
| <kbd>Ctrl</kbd> + <kbd>D</kbd>      | Quitter le shell                                                    |
| `man <commande>`                    | Le **manuel** complet (<kbd>q</kbd> pour quitter)                   |
| `<commande> --help`                 | Aide rapide                                                         |

&#x20;

{% hint style="success" %}
**La touche Tab est ta meilleure amie.** Elle évite les fautes de frappe et te montre ce qui existe. Les pros ne tapent presque jamais un chemin en entier.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">03</mark> · Où suis-je ? `pwd`

&#x20;

```
$ pwd
/home/alice
```

&#x20;

`pwd` (_print working directory_), c'est le point rouge **« Vous êtes ici »** sur le plan du centre commercial.

&#x20;

***

&#x20;

## <mark style="color:purple;">04</mark> · Qu'y a-t-il ici ? `ls`

&#x20;

{% tabs %}
{% tab title="Basique" %}
```bash
ls              # contenu du dossier courant
ls /etc         # contenu d'un autre dossier
```
{% endtab %}

{% tab title="Détails" %}
```bash
ls -l           # format long
ls -lh          # tailles lisibles (K, M, G)
ls -lt          # trié par date de modification
```
{% endtab %}

{% tab title="Cachés" %}
```bash
ls -a           # aussi les fichiers cachés (qui commencent par .)
ls -la          # cachés + détails
```
{% endtab %}

{% tab title="Récursif" %}
```bash
ls -R           # tous les sous-dossiers
```
{% endtab %}
{% endtabs %}

&#x20;

### Lire la sortie de `ls -l`

&#x20;

```
-rw-r--r--  1  alice  staff  1.2K  Sep 14 10:32  notes.txt
```

&#x20;

| Morceau            | Signification                                                                 |
| ------------------ | ----------------------------------------------------------------------------- |
| `-rw-r--r--`       | **Type** (`-` fichier, `d` dossier, `l` lien) + **permissions** ([chap. 7](07-permissions.md)) |
| `1`                | Nombre de liens                                                                |
| `alice`            | **Propriétaire**                                                               |
| `staff`            | **Groupe**                                                                     |
| `1.2K`             | **Taille**                                                                     |
| `Sep 14 10:32`     | Dernière **modification**                                                      |
| `notes.txt`        | **Nom**                                                                        |

&#x20;

{% hint style="info" %}
Les **fichiers cachés** commencent par un point : `.bashrc`, `.zshrc`, `.gitignore`… Ce sont souvent des fichiers de configuration.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">05</mark> · Se déplacer : `cd`

&#x20;

```mermaid
flowchart TB
    R["/"] --> H["home"]
    H --> A["alice"]
    A --> D["Documents"]

    D -->|"cd .."| A
    A -->|"cd Documents"| D
    D -->|"cd /"| R
    R -->|"cd ~"| A

    style A fill:#dcfce7,stroke:#22c55e,color:#14532d
    style R fill:#fef3c7,stroke:#f59e0b,color:#78350f
```

&#x20;

```bash
cd /etc              # chemin absolu
cd Documents         # chemin relatif
cd ..                # remonter d'un niveau
cd ../..             # remonter de deux niveaux
cd ~                 # aller dans son home   (ou simplement : cd)
cd -                 # revenir au dossier précédent
```

&#x20;

***

&#x20;

## <mark style="color:purple;">06</mark> · Créer, copier, déplacer

&#x20;

{% columns %}
{% column %}
### Créer

```bash
mkdir projets        # un dossier
mkdir -p a/b/c       # toute une hiérarchie
touch notes.txt      # un fichier vide
```
{% endcolumn %}

{% column %}
### Copier

```bash
cp notes.txt copie.txt
cp notes.txt Documents/
cp -r projets/ sauvegarde/   # dossier : -r !
```
{% endcolumn %}

{% column %}
### Déplacer / renommer

```bash
mv notes.txt Documents/
mv ancien.txt nouveau.txt
```
{% endcolumn %}
{% endcolumns %}

&#x20;

{% hint style="info" %}
**Il n'y a pas de commande « renommer »** : renommer, c'est **déplacer vers un nouveau nom**, avec `mv`.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">07</mark> · Supprimer

&#x20;

```bash
rm notes.txt         # un fichier
rm -i notes.txt      # demande confirmation avant
rmdir vide/          # un dossier VIDE
rm -r projets/       # un dossier et tout son contenu
```

&#x20;

{% hint style="danger" %}
**Pas de corbeille dans le terminal.** Un fichier supprimé avec `rm` est **définitivement perdu**.

&#x20;

Méfie-toi surtout de `rm -rf` (`-r` récursif, `-f` sans confirmation). Une faute de frappe comme `rm -rf / home/alice/tmp` (espace en trop) tente d'effacer **tout le système**. Relis toujours deux fois.
{% endhint %}

&#x20;

```mermaid
flowchart TD
    S(["Je veux supprimer"]) --> Q1{"Fichier ou dossier ?"}
    Q1 -->|Fichier| F["rm fichier"]
    Q1 -->|Dossier| Q2{"Il est vide ?"}
    Q2 -->|Oui| V["rmdir dossier"]
    Q2 -->|Non| Q3{"J'ai vérifié avec ls ?"}
    Q3 -->|Non| L["ls dossier d'abord !"]
    L --> Q3
    Q3 -->|Oui| R["rm -r dossier"]

    style F fill:#dcfce7,stroke:#22c55e,color:#14532d
    style V fill:#dcfce7,stroke:#22c55e,color:#14532d
    style R fill:#fef3c7,stroke:#f59e0b,color:#78350f
    style L fill:#fee2e2,stroke:#ef4444,color:#7f1d1d
```

&#x20;

***

&#x20;

## <mark style="color:purple;">08</mark> · Les jokers (wildcards)

&#x20;

| Joker   | Signification                       | Exemple            | Correspond à                          |
| ------- | ----------------------------------- | ------------------ | ------------------------------------- |
| `*`     | N'importe quelle suite de caractères | `*.txt`            | `notes.txt`, `a.txt`, `todo.txt`      |
| `?`     | Un seul caractère                   | `photo?.jpg`       | `photo1.jpg`, `photoA.jpg`            |
| `[...]` | Un caractère parmi une liste        | `rapport[12].pdf`  | `rapport1.pdf`, `rapport2.pdf`        |

&#x20;

```bash
ls *.txt             # tous les .txt
cp *.jpg Photos/     # copier toutes les images
rm brouillon*        # tout ce qui commence par "brouillon"
```

&#x20;

{% hint style="warning" %}
**Avant un `rm` avec un joker**, fais d'abord un `ls` avec le **même** motif pour voir ce qui sera supprimé.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">09</mark> · Trouver et identifier

&#x20;

{% columns %}
{% column %}
### `find`

```bash
find . -name "*.txt"
find /home -name "notes*"
find . -type d
find . -size +100M
```
{% endcolumn %}

{% column %}
### Infos utiles

```bash
whoami            # qui suis-je ?
which ls          # où est ls ?
file notes.txt    # quel type ?
du -sh Documents  # taille d'un dossier
df -h             # espace disque
```
{% endcolumn %}
{% endcolumns %}

&#x20;

***

&#x20;

## <mark style="color:purple;">10</mark> · En résumé

&#x20;

| Besoin                          | Commande                              |
| ------------------------------- | ------------------------------------- |
| Où suis-je ?                 | `pwd`                                 |
| Qu'y a-t-il ici ?            | `ls -la`                              |
| Aller ailleurs               | `cd <chemin>`                         |
| Créer                        | `mkdir` / `touch`                     |
| Copier / déplacer         | `cp` / `mv`                           |
| Supprimer                    | `rm` (dossier : `rm -r`) |
| Trouver                      | `find`                                |
| Aide                         | `man <commande>` / `--help`           |

&#x20;

<mark style="color:green;">**→ Suite :**</mark> [4. Lire et chercher du texte](04-lire-chercher-texte.md)
