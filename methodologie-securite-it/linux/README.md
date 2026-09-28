---
description: Linux et la ligne de commande — comprendre le système et le piloter au clavier.
---

# 🐧 Linux & Shell

> _Une interface graphique te montre ce qu'on a prévu pour toi.
> Le terminal te laisse faire tout ce que le système sait faire._

## Ce que tu vas apprendre

À la fin de ce chapitre, tu sauras :

1. **Ce qu'est Linux**, d'où il vient, et ce qu'est un **shell**
2. **Comment sont rangés les dossiers** sous Linux (`/`, `/home`, `/usr`, `/etc`…)
3. **Te déplacer et manipuler des fichiers** en ligne de commande
4. **Lire et chercher dans du texte** : `cat`, `less`, `grep`, `head`, `tail`…
5. **Rediriger et enchaîner des commandes** avec `>`, `>>`, `<` et les pipes `|`
6. **Utiliser les variables d'environnement** comme `$PATH` et `$HOME`
7. **Gérer les utilisateurs, groupes et permissions** (`chmod`, `chown`)
8. **Connaître les principaux systèmes de fichiers** (ext4, Btrfs, tmpfs, `/proc`…)

## Plan du chapitre

| # | Page | Idée clé |
| --- | --- | --- |
| 1 | [Introduction](01-introduction.md) | Histoire de Linux, noyau, shell, types de shells |
| 2 | [L'arborescence](02-arborescence.md) | Tout part de `/` : le rôle de chaque dossier |
| 3 | [Naviguer et gérer les fichiers](03-navigation-fichiers.md) | `pwd`, `ls`, `cd`, `mkdir`, `cp`, `mv`, `rm`… |
| 4 | [Lire et chercher du texte](04-lire-chercher-texte.md) | `cat`, `less`, `head`, `tail`, `echo`, `grep`, `wc`, `sort` |
| 5 | [Redirections et pipes](05-redirections-pipes.md) | stdin, stdout, stderr, `>`, `>>`, `<`, `\|` |
| 6 | [Variables d'environnement](06-variables-environnement.md) | `$PATH`, `$HOME`, `export`, `.zshrc` |
| 7 | [Utilisateurs et permissions](07-permissions.md) | `rwx`, `chmod`, `chown`, moindre privilège |
| 8 | [Systèmes de fichiers](08-systemes-de-fichiers.md) | ext4, XFS, Btrfs, tmpfs, procfs, sysfs |
| 9 | [Aide-mémoire](09-aide-memoire.md) | Toutes les commandes sur une page |
| 10 | [Questions de révision](10-questions-revision.md) | Vérifier que tout est compris |

## Préparer son environnement

Pour pratiquer, il te faut un shell Linux :

{% tabs %}
{% tab title="🐧 Linux" %}
Rien à faire : ouvre simplement un **terminal**.
{% endtab %}

{% tab title="🍎 macOS" %}
Ouvre l'application **Terminal**. macOS est un système Unix (famille BSD) : la plupart des commandes sont identiques, à quelques options près.
{% endtab %}

{% tab title="🪟 Windows" %}
Installe **WSL** (Windows Subsystem for Linux), qui fait tourner un vrai Linux dans Windows :

```powershell
wsl --install
```

📖 [Guide d'installation de WSL](https://learn.microsoft.com/fr-fr/windows/wsl/install)
{% endtab %}

{% tab title="🐳 Docker" %}
Si Docker est installé, lance un shell zsh jetable dans un conteneur :

```bash
docker run -it ohmyzsh/zsh
```

Parfait pour expérimenter sans risquer de casser ta machine. Voir le chapitre [Docker](../docker/README.md).
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Conseil :** tape toutes les commandes de ce chapitre toi-même. La ligne de commande s'apprend avec les doigts, pas avec les yeux.
{% endhint %}

## Références

* _Modern Operating Systems_, Andrew S. Tanenbaum & Herbert Bos
* [Introduction to Bash and Bash scripting — GeeksforGeeks](https://www.geeksforgeeks.org/bash-scripting-introduction-to-bash-and-bash-scripting/)
* [Basic shell commands in Linux — GeeksforGeeks](https://www.geeksforgeeks.org/linux-unix/basic-shell-commands-in-linux/)
* [Linux shells — phoenixNAP](https://phoenixnap.com/kb/linux-shells)
* [Linux user groups and permissions guide — daily.dev](https://daily.dev/blog/linux-user-groups-and-permissions-guide/)
* [How do Zsh configuration files work — freeCodeCamp](https://www.freecodecamp.org/news/how-do-zsh-configuration-files-work/)
* [History of Unix & Linux — FrontPageLinux](https://frontpagelinux.com/articles/guide-through-history-of-unix-linux-everything-you-need-to-know/)

_Source : cours « IT Methodology — Shell / Command Line », P. Rétornaz & A. Jungo, HEIA-FR._
