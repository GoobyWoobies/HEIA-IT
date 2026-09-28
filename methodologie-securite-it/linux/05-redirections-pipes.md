---
description: Les flux stdin, stdout et stderr, les redirections >, >>, <, 2> et les pipes |.
---

# 5. Redirections et pipes

C'est ici que la ligne de commande devient vraiment puissante : au lieu d'une commande isolée, tu vas **brancher des commandes entre elles** comme des tuyaux.

## Tout est fichier… et chaque fichier a un numéro

Sous Linux, **tout est fichier**. Quand un programme ouvre un fichier, le système lui donne un petit numéro pour s'y référer : un **descripteur de fichier** (_file descriptor_, FD).

Chaque programme démarre avec **trois descripteurs déjà ouverts** :

| Flux | Descripteur | Nom | Par défaut |
| --- | --- | --- | --- |
| Entrée standard | **0** | `stdin` | Le **clavier** |
| Sortie standard | **1** | `stdout` | L'**écran** |
| Erreur standard | **2** | `stderr` | L'**écran** |

```
                     ┌──────────────────────┐
  Clavier ──stdin(0)─►   n'importe quelle   ├──stdout(1)──► Écran  (résultats)
                     │   commande           │
                     │                      ├──stderr(2)──► Écran  (erreurs)
                     └──────────────────────┘
```

Exemple :

```bash
$ ls /                                     # résultat → stdout (écran)
bin  boot  dev  etc  home  lib  ...

$ ls aaa                                   # erreur → stderr (écran aussi)
ls: cannot access 'aaa': No such file or directory
```

Les deux s'affichent à l'écran, mais ce sont **deux canaux différents**. On va pouvoir les séparer.

### 🚰 L'analogie de la plomberie

Un programme est comme un **appareil de plomberie** avec un **tuyau d'arrivée** (stdin) et **deux tuyaux de sortie** : l'eau propre (stdout) et l'évacuation (stderr). Par défaut, tout se déverse dans l'évier (l'écran). Les redirections permettent de **rebrancher les tuyaux** ailleurs.

## Les redirections

### `>` : envoyer la sortie dans un fichier

```bash
ls > liste.txt
```

Le résultat de `ls` va dans `liste.txt` au lieu de l'écran. Le fichier est **créé**, ou **écrasé** s'il existe déjà.

{% hint style="danger" %}
`>` **écrase** le contenu existant sans prévenir. `echo "test" > important.txt` remplace tout le fichier par le mot « test ».
{% endhint %}

### `>>` : ajouter à la fin d'un fichier

```bash
ls >> liste.txt
echo "nouvelle ligne" >> journal.txt
```

Le résultat est **ajouté à la fin** du fichier, sans effacer ce qui existe.

### `2>` : rediriger les erreurs

```bash
ls aaa 2> erreurs.txt
```

Le message d'erreur va dans `erreurs.txt`, l'écran reste propre. Le `2` désigne le descripteur de **stderr**.

### `<` : lire l'entrée depuis un fichier

```bash
cat < fruits.txt                    # cat lit fruits.txt comme si on le tapait au clavier
cat < fruits.txt > copie.txt        # lit fruits.txt, écrit dans copie.txt
wc -l < fruits.txt                  # compte les lignes
```

### Combinaisons utiles

```bash
commande > sortie.txt 2> erreurs.txt   # séparer résultats et erreurs
commande > tout.txt 2>&1               # tout dans le même fichier (2 vers là où va 1)
commande 2> /dev/null                  # jeter les erreurs
commande > /dev/null 2>&1              # ne rien afficher du tout
```

{% hint style="info" %}
**`/dev/null`** est un « trou noir » : tout ce qu'on y écrit disparaît. Parfait pour faire taire une commande bavarde.

Exemple : `find / -name "*.conf" 2> /dev/null` affiche les résultats sans la centaine de « Permission denied ».
{% endhint %}

### Récapitulatif

| Symbole | Effet | Exemple |
| --- | --- | --- |
| `>` | stdout → fichier (**écrase**) | `ls > liste.txt` |
| `>>` | stdout → fichier (**ajoute**) | `ls >> liste.txt` |
| `2>` | stderr → fichier | `ls aaa 2> err.txt` |
| `2>&1` | stderr → même endroit que stdout | `cmd > log.txt 2>&1` |
| `<` | fichier → stdin | `cat < fruits.txt` |

## Les pipes : `|`

Un **pipe** (tube) envoie la **sortie d'une commande directement à l'entrée de la suivante**. Pas de fichier intermédiaire.

```
  ┌──────┐  stdout → stdin  ┌──────┐  stdout → stdin  ┌──────┐
  │  ls  ├────────|────────►│ sort ├────────|────────►│  wc  ├──► écran
  └──────┘                  └──────┘                  └──────┘
```

Le caractère pipe est **`|`** (<kbd>AltGr</kbd> + <kbd>7</kbd> sur un clavier suisse, <kbd>AltGr</kbd> + <kbd>6</kbd> sur un clavier français).

### 🏭 L'analogie de la chaîne de montage

Chaque commande est un **ouvrier** sur une chaîne de montage. Il fait **une seule tâche**, puis passe le résultat au suivant. C'est la philosophie Unix : de petits outils simples qu'on assemble.

### Exemples

```bash
ls | grep test.txt              # lister, puis ne garder que test.txt
ls | sort -r                    # lister, puis trier à l'envers
ls | sort -r | wc -l            # … puis compter les lignes
```

### Des pipelines utiles dans la vraie vie

```bash
# Combien de fichiers .txt dans ce dossier ?
ls | grep ".txt" | wc -l

# Les 5 fruits les plus fréquents
sort fruits.txt | uniq -c | sort -rn | head -5

# Les erreurs récentes dans les logs
cat /var/log/syslog | grep -i error | tail -20

# Parcourir confortablement une longue sortie
ls -la /etc | less

# Trouver un processus en cours
ps aux | grep firefox

# Rechercher dans l'historique de commandes
history | grep docker
```

Décomposons `sort fruits.txt | uniq -c | sort -rn | head -5` :

| Étape | Commande | Résultat |
| --- | --- | --- |
| 1 | `sort fruits.txt` | Trie les lignes pour regrouper les doublons |
| 2 | `uniq -c` | Compte chaque ligne : `2 pomme`, `1 kiwi`… |
| 3 | `sort -rn` | Trie par nombre, du plus grand au plus petit |
| 4 | `head -5` | Garde les 5 premiers |

{% hint style="success" %}
**Construis tes pipelines petit à petit.** Lance la première commande, regarde le résultat, ajoute `| la suivante`, regarde encore… C'est comme ça que tout le monde fait.
{% endhint %}

## Redirection vs pipe

| | Redirection `>` | Pipe `\|` |
| --- | --- | --- |
| Envoie la sortie vers… | un **fichier** | une **autre commande** |
| Exemple | `ls > liste.txt` | `ls \| wc -l` |

## En résumé

* Chaque programme a **3 flux** : stdin (0), stdout (1), stderr (2).
* `>` écrase, `>>` ajoute, `2>` redirige les erreurs, `<` lit depuis un fichier.
* `/dev/null` fait disparaître ce qu'on lui envoie.
* `|` branche la sortie d'une commande sur l'entrée de la suivante.
