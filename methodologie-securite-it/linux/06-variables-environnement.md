---
description: Les variables d'environnement, le PATH, et les fichiers de configuration du shell.
---

# 6. Variables d'environnement

## Qu'est-ce que c'est ?

Une **variable d'environnement** est une valeur nommée que le shell garde en mémoire et **transmet aux programmes qu'il lance** (ses processus enfants). Elles définissent de nombreux aspects du fonctionnement du système : qui tu es, où est ton dossier personnel, où chercher les programmes…

### 🎒 L'analogie du sac à dos

Imagine que chaque programme part en excursion avec un **sac à dos** préparé par le shell. Dedans : une carte (`PATH`), l'adresse de la maison (`HOME`), le nom du randonneur (`USER`)… Tous les programmes lancés depuis ce shell reçoivent **une copie** de ce sac.

## Afficher les variables

```bash
printenv              # toutes les variables d'environnement
env                   # idem
echo $HOME            # la valeur d'une seule variable (avec $)
printenv HOME         # idem, sans $
```

```
$ printenv
SHELL=/bin/bash
HOME=/root
PWD=/root
USER=root
PATH=/usr/sbin:/usr/bin:/sbin:/bin
```

## Les variables à connaître

| Variable | Contenu | Exemple |
| --- | --- | --- |
| `HOME` | Ton dossier personnel | `/home/alice` |
| `USER` | Ton nom d'utilisateur | `alice` |
| `SHELL` | Ton shell par défaut | `/bin/zsh` |
| `PWD` | Le dossier courant | `/home/alice/projets` |
| `PATH` | Les dossiers où chercher les programmes | `/usr/local/bin:/usr/bin:/bin` |
| `LANG` | La langue et l'encodage | `fr_CH.UTF-8` |
| `EDITOR` | L'éditeur de texte par défaut | `nano`, `vim` |

## Le `PATH` : où sont les programmes ?

Quand tu tapes `ls`, comment le shell sait-il **où** se trouve ce programme ? Il regarde la variable **`PATH`** : une liste de dossiers séparés par des `:`, parcourus **dans l'ordre**.

```
$ echo $PATH
/usr/local/bin:/usr/bin:/bin

  Je tape "ls" →  1. /usr/local/bin/ls ?  ✗ absent
                  2. /usr/bin/ls ?        ✓ trouvé ! on l'exécute
```

```bash
which ls          # affiche le chemin du programme qui sera utilisé → /usr/bin/ls
```

Si le programme n'est dans **aucun** dossier du `PATH`, tu obtiens :

```
zsh: command not found: monscript
```

### 📚 L'analogie de la bibliothèque

Le `PATH`, c'est la **liste des étagères** où le bibliothécaire va chercher un livre, **dans l'ordre**. S'il le trouve sur la première étagère, il ne regarde pas les autres.

{% hint style="info" %}
C'est pour ça qu'on lance un script du dossier courant avec **`./script.sh`** : le dossier courant `.` n'est **pas** dans le `PATH` (par sécurité), il faut donc donner le chemin explicitement.
{% endhint %}

### Modifier le `PATH`

```bash
PATH=$PATH:/usr/local/bin     # ajoute /usr/local/bin À LA FIN du PATH
echo $PATH
# /usr/sbin:/usr/bin:/sbin:/bin:/usr/local/bin
```

`$PATH:/usr/local/bin` signifie « l'ancienne valeur, suivie de `:/usr/local/bin` ».

{% hint style="danger" %}
N'écris **jamais** `PATH=/mon/dossier` sans `$PATH:` devant : tu **remplaces** toute la liste, et plus aucune commande (`ls`, `cat`…) n'est trouvée. Si ça t'arrive, ferme simplement le terminal et rouvre-le.
{% endhint %}

## Créer ses variables : `export`

```bash
PRENOM="Alice"            # variable du shell (locale, PAS d'espaces autour du =)
echo $PRENOM              # → Alice

export PRENOM="Alice"     # variable d'ENVIRONNEMENT : transmise aux programmes lancés
export EDITOR=nano

unset PRENOM              # supprime la variable
```

{% hint style="warning" %}
**Pas d'espace autour du `=`** : `NOM="Alice"` fonctionne, `NOM = "Alice"` provoque une erreur (le shell croit que `NOM` est une commande).
{% endhint %}

| | Variable du shell | Variable d'environnement |
| --- | --- | --- |
| Création | `NOM=valeur` | `export NOM=valeur` |
| Visible dans le shell courant | ✅ | ✅ |
| Transmise aux programmes lancés | ❌ | ✅ |

## Rendre les changements permanents

Une variable définie dans le terminal **disparaît quand tu le fermes**. Pour qu'elle soit définie à chaque ouverture, on l'écrit dans un **fichier de configuration** du shell, lu au démarrage.

| Shell | Fichiers principaux |
| --- | --- |
| **bash** | `~/.bashrc`, `~/.bash_profile` |
| **zsh** | `~/.zshenv`, `~/.zprofile`, `~/.zshrc`, `~/.zlogin` |

Pour zsh, l'ordre et le rôle :

| Fichier | Quand est-il lu ? | À utiliser pour |
| --- | --- | --- |
| `.zshenv` | **Toujours**, en premier | Variables nécessaires partout |
| `.zprofile` | Shell de connexion (_login_) | Configuration au démarrage de session |
| `.zshrc` | Shell **interactif** (chaque terminal ouvert) | Alias, prompt, `PATH`, plugins — **le plus utilisé** |
| `.zlogin` | Shell de connexion, après `.zshrc` | Rarement utilisé |

```bash
# Ajouter une ligne à la fin de ~/.zshrc
echo 'export PATH=$PATH:$HOME/bin' >> ~/.zshrc

# Recharger la configuration sans fermer le terminal
source ~/.zshrc
```

📖 [How do Zsh configuration files work? — freeCodeCamp](https://www.freecodecamp.org/news/how-do-zsh-configuration-files-work/)

## Bonus : les alias

Un **alias** est un raccourci pour une commande longue :

```bash
alias ll='ls -lah'
alias gs='git status'
```

Mets-les dans ton `~/.zshrc` (ou `~/.bashrc`) pour les garder.

## En résumé

* Les **variables d'environnement** configurent le shell et sont transmises aux programmes lancés.
* `printenv` pour tout voir, `echo $NOM` pour une seule.
* **`PATH`** = liste ordonnée des dossiers où chercher les commandes. On ajoute avec `PATH=$PATH:/nouveau`.
* `export` rend une variable visible aux programmes enfants.
* Pour que ce soit permanent : l'écrire dans `~/.zshrc` ou `~/.bashrc`, puis `source`.
