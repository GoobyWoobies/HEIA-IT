---
description: Afficher, parcourir, filtrer et compter du texte avec cat, less, head, tail, echo, grep, wc et sort.
icon: magnifying-glass
cover: https://placehold.co/1600x500/0f172a/4ade80?text=Linux+%C2%B7+Lire+et+chercher
coverY: 0
---

# 4. Lire et chercher du texte

<mark style="color:blue;">**Sous Linux, presque tout est du texte. Autant savoir le lire.**</mark>

&#x20;

{% hint style="info" %}
**En bref**

`cat` affiche tout, `less` feuillette, `head` et `tail` montrent le début et la fin, `grep` filtre, `wc` compte, `sort` trie. Seuls, ils sont utiles ; combinés avec des pipes, ils deviennent redoutables.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Quel outil choisir ?

&#x20;

```mermaid
flowchart TD
    S(["Je veux…"]) --> A{"…voir le fichier ?"}
    S --> B{"…trouver quelque chose ?"}
    S --> C{"…compter ou trier ?"}
    A -->|"petit"| CAT["cat"]
    A -->|"gros"| LESS["less"]
    A -->|"le début"| HEAD["head"]
    A -->|"la fin / en direct"| TAIL["tail · tail -f"]
    B --> GREP["grep"]
    C -->|"compter"| WC["wc -l"]
    C -->|"trier / dédoublonner"| SORT["sort · uniq"]

    style CAT fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style LESS fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style HEAD fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style TAIL fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style GREP fill:#fce7f3,stroke:#ec4899,color:#831843
    style WC fill:#dcfce7,stroke:#22c55e,color:#14532d
    style SORT fill:#dcfce7,stroke:#22c55e,color:#14532d
```

&#x20;

Pour les exemples, on utilise ce fichier `fruits.txt` :

&#x20;

```
pomme
banane
cerise
pomme
kiwi
Banane
abricot
```

&#x20;

***

&#x20;

## <mark style="color:purple;">02</mark> · Afficher : `echo`

&#x20;

```bash
echo "Bonjour le monde"       # → Bonjour le monde
echo $HOME                    # → /home/alice
echo "Je suis $USER"          # → Je suis alice
echo "ligne" > fichier.txt    # écrire dans un fichier (chap. 5)
```

&#x20;

{% columns %}
{% column %}
**`"guillemets doubles"`**

Les variables sont **remplacées**.

`echo "$HOME"` → `/home/alice`
{% endcolumn %}

{% column %}
**`'guillemets simples'`**

Tout est pris **littéralement**.

`echo '$HOME'` → `$HOME`
{% endcolumn %}
{% endcolumns %}

&#x20;

***

&#x20;

## <mark style="color:purple;">03</mark> · Lire : `cat` et `less`

&#x20;

{% columns %}
{% column %}
### `cat`

Affiche **tout d'un coup**.

```bash
cat fruits.txt
cat -n fruits.txt       # numéros de ligne
cat a.txt b.txt         # à la suite
cat a.txt b.txt > ab.txt
```

Parfait pour les **petits** fichiers.
{% endcolumn %}

{% column %}
### `less`

Affiche **page par page**, avec recherche.

```bash
less /var/log/syslog
```

Parfait pour les **gros** fichiers.
{% endcolumn %}
{% endcolumns %}

&#x20;

### L'analogie du livre

&#x20;

* `cat`, c'est **jeter toutes les pages du livre sur la table** d'un coup.
* `less`, c'est **lire le livre page par page**, avec un index pour chercher.

&#x20;

### Naviguer dans `less`

&#x20;

| Touche                              | Action                                  |
| ----------------------------------- | --------------------------------------- |
| <kbd>Espace</kbd> / <kbd>b</kbd>    | Page suivante / précédente              |
| <kbd>↑</kbd> / <kbd>↓</kbd>         | Ligne par ligne                         |
| <kbd>g</kbd> / <kbd>G</kbd>         | Début / fin                             |
| `/mot`                              | <mark style="color:blue;">**Chercher**</mark> « mot » |
| <kbd>n</kbd> / <kbd>N</kbd>         | Occurrence suivante / précédente        |
| <kbd>q</kbd>                        | <mark style="color:green;">**Quitter**</mark>          |

&#x20;

{% hint style="info" %}
**« less is more »** — `less` est une version améliorée d'un ancien outil nommé `more`, qui ne savait qu'avancer. D'où le jeu de mots.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">04</mark> · Le début ou la fin : `head` et `tail`

&#x20;

```bash
head fruits.txt               # les 10 premières lignes
head -n 3 fruits.txt          # les 3 premières
tail fruits.txt               # les 10 dernières
tail -n 2 fruits.txt          # les 2 dernières
tail -f /var/log/syslog       # suit le fichier EN DIRECT (Ctrl+C pour arrêter)
```

&#x20;

{% hint style="success" %}
**`tail -f`** est l'outil n°1 pour **surveiller des logs** : chaque nouvelle ligne s'affiche dès qu'elle est écrite.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">05</mark> · Chercher : `grep`

&#x20;

`grep` affiche **uniquement les lignes qui contiennent un motif**. C'est l'un des outils les plus utilisés sous Linux.

&#x20;

```mermaid
flowchart LR
    IN["pomme<br/>banane<br/>cerise<br/>pomme<br/>kiwi"] -->|"grep pomme"| OUT["pomme<br/>pomme"]

    style IN fill:#f8fafc,stroke:#64748b,color:#0f172a
    style OUT fill:#dcfce7,stroke:#22c55e,color:#14532d
```

&#x20;

**L'analogie du surligneur** : `grep`, c'est passer un surligneur sur un document et **ne garder que les lignes surlignées**.

&#x20;

### Les options essentielles

&#x20;

| Option | Effet                                             | Exemple                                         |
| ------ | ------------------------------------------------- | ----------------------------------------------- |
| `-i`   | Ignore la **casse**                               | `grep -i banane` → `banane` **et** `Banane`     |
| `-n`   | Affiche le **numéro** de ligne                    | `grep -n kiwi` → `5:kiwi`                       |
| `-v`   | **Inverse** : lignes **sans** le motif            | `grep -v pomme`                                 |
| `-c`   | **Compte** les lignes trouvées                    | `grep -c pomme` → `2`                           |
| `-r`   | Cherche dans tout un **dossier**                  | `grep -r "TODO" src/`                           |
| `-l`   | Seulement le **nom des fichiers**                 | `grep -rl "password" .`                         |
| `-w`   | **Mot entier** uniquement                         | `grep -w an` ne trouve pas « banane »           |

&#x20;

{% tabs %}
{% tab title="Logs" %}
```bash
grep -i "error" /var/log/syslog
```

Toutes les erreurs, peu importe la casse.
{% endtab %}

{% tab title="Code" %}
```bash
grep -rn "TODO" .
```

Tous les TODO du projet, avec fichier et numéro de ligne.
{% endtab %}

{% tab title="Config" %}
```bash
grep -v "^#" /etc/ssh/sshd_config
```

La configuration sans les lignes de commentaire.
{% endtab %}
{% endtabs %}

&#x20;

<details>

<summary>Pour aller plus loin : les expressions régulières</summary>

&#x20;

`grep` comprend des motifs appelés **expressions régulières** (_regex_) :

&#x20;

| Motif | Signification                          | Exemple                                   |
| ----- | -------------------------------------- | ----------------------------------------- |
| `^`   | Début de ligne                         | `grep "^b"` → lignes qui commencent par b |
| `$`   | Fin de ligne                           | `grep "e$"` → lignes qui finissent par e  |
| `.`   | N'importe quel caractère               | `grep "k.w"` → `kiwi`                     |
| `*`   | Le caractère précédent, 0 fois ou plus | `grep "po*"`                              |

&#x20;

</details>

&#x20;

***

&#x20;

## <mark style="color:purple;">06</mark> · Compter, trier, comparer

&#x20;

{% columns %}
{% column %}
### `wc`

```bash
wc fruits.txt       # lignes mots octets
wc -l fruits.txt    # → 7
wc -w fruits.txt    # mots
```
{% endcolumn %}

{% column %}
### `sort` · `uniq`

```bash
sort fruits.txt
sort -r fruits.txt       # inversé
sort -n nombres.txt      # numérique
sort fruits.txt | uniq
sort fruits.txt | uniq -c
```
{% endcolumn %}

{% column %}
### `diff`

```bash
diff ancien.txt nouveau.txt
```

Affiche les lignes qui diffèrent.
{% endcolumn %}
{% endcolumns %}

&#x20;

{% hint style="warning" %}
**`uniq` seul ne suffit pas** — il ne supprime que les doublons **consécutifs**. Il faut presque toujours **trier avant** : `sort | uniq`.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">07</mark> · En résumé

&#x20;

| Besoin                              | Commande                                  |
| ----------------------------------- | ----------------------------------------- |
| Afficher un message / une variable | `echo`                                  |
| Afficher un petit fichier        | `cat`                                     |
| Parcourir un gros fichier        | `less` (<kbd>q</kbd> pour quitter)        |
| Début / fin                   | `head` / `tail` (`-f` pour suivre)        |
| Filtrer des lignes               | `grep` (`-i`, `-n`, `-v`, `-r`)           |
| Compter                          | `wc -l`                                   |
| Trier / dédoublonner             | `sort` / `sort \| uniq`                   |

&#x20;

La vraie puissance arrive quand on **combine** ces outils : c'est le chapitre suivant.

&#x20;

<mark style="color:green;">**→ Suite :**</mark> [5. Redirections et pipes](05-redirections-pipes.md)
