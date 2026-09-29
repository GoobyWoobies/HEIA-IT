---
description: Linux et la ligne de commande — comprendre le système et le piloter au clavier.
icon: linux
cover: https://placehold.co/1600x500/0f172a/4ade80?text=Linux+%26+Shell
coverY: 0
---

# Linux & Shell

<mark style="color:blue;">**Parler directement à ta machine, sans intermédiaire.**</mark>

&#x20;

> _Une interface graphique te montre ce qu'on a prévu pour toi._
> _Le terminal te laisse faire tout ce que le système sait faire._

&#x20;

{% hint style="info" %}
**En bref**

Linux fait tourner la majorité des serveurs, du cloud, des conteneurs Docker et des smartphones Android. Le **shell** est la porte d'entrée pour le piloter. Quelques dizaines de commandes suffisent pour être à l'aise.
{% endhint %}

&#x20;

<figure><img src="https://upload.wikimedia.org/wikipedia/commons/3/35/Tux.svg" alt="Tux, la mascotte de Linux" width="200"><figcaption><p>Tux, le manchot mascotte de Linux.</p></figcaption></figure>

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Ce que tu vas apprendre

&#x20;

* <mark style="color:blue;">**Ce qu'est Linux**</mark>, d'où il vient, et ce qu'est un **shell**
* <mark style="color:blue;">**Comment sont rangés les dossiers**</mark> : `/`, `/home`, `/usr`, `/etc`…
* <mark style="color:blue;">**Te déplacer et manipuler des fichiers**</mark> au clavier
* <mark style="color:blue;">**Lire et chercher dans du texte**</mark> : `cat`, `less`, `grep`, `head`, `tail`…
* <mark style="color:blue;">**Brancher des commandes entre elles**</mark> avec `>`, `>>`, `<` et `|`
* <mark style="color:blue;">**Configurer ton environnement**</mark> : `$PATH`, `$HOME`, `.zshrc`
* <mark style="color:blue;">**Gérer les droits**</mark> : utilisateurs, groupes, `chmod`, `chown`
* <mark style="color:blue;">**Connaître les systèmes de fichiers**</mark> : ext4, Btrfs, tmpfs, `/proc`…

&#x20;

***

&#x20;

## <mark style="color:purple;">02</mark> · Ton parcours

&#x20;

```mermaid
flowchart LR
    A["1. Introduction"] --> B["2. Arborescence"]
    B --> C["3. Navigation"]
    C --> D["4. Lire & chercher"]
    D --> E["5. Pipes"]
    E --> F["6. Variables"]
    F --> G["7. Permissions"]
    G --> H["8. Filesystems"]
    H --> I["Révision"]

    style A fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style B fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style C fill:#ede9fe,stroke:#8b5cf6,color:#4c1d95
    style D fill:#ede9fe,stroke:#8b5cf6,color:#4c1d95
    style E fill:#fce7f3,stroke:#ec4899,color:#831843
    style F fill:#fce7f3,stroke:#ec4899,color:#831843
    style G fill:#fef3c7,stroke:#f59e0b,color:#78350f
    style H fill:#fef3c7,stroke:#f59e0b,color:#78350f
    style I fill:#dcfce7,stroke:#22c55e,color:#14532d
```

&#x20;

| #  | Page                                                         | L'idée clé en une phrase                              |
| -- | ------------------------------------------------------------ | ----------------------------------------------------- |
| 1  | [Introduction](01-introduction.md)                           | Unix, GNU, Linux ; noyau et shell                     |
| 2  | [L'arborescence](02-arborescence.md)                         | Tout part de `/` : le rôle de chaque dossier          |
| 3  | [Naviguer et gérer les fichiers](03-navigation-fichiers.md)  | `pwd`, `ls`, `cd`, `mkdir`, `cp`, `mv`, `rm`…         |
| 4  | [Lire et chercher du texte](04-lire-chercher-texte.md)       | `cat`, `less`, `head`, `tail`, `grep`, `wc`, `sort`   |
| 5  | [Redirections et pipes](05-redirections-pipes.md)            | stdin, stdout, stderr, `>`, `>>`, `<`, `\|`           |
| 6  | [Variables d'environnement](06-variables-environnement.md)   | `$PATH`, `$HOME`, `export`, `.zshrc`                  |
| 7  | [Utilisateurs et permissions](07-permissions.md)             | `rwx`, `chmod`, `chown`, moindre privilège            |
| 8  | [Systèmes de fichiers](08-systemes-de-fichiers.md)           | ext4, XFS, Btrfs, tmpfs, procfs, sysfs                |
| 9  | [Aide-mémoire](09-aide-memoire.md)                           | Toutes les commandes sur une page                     |
| 10 | [Questions de révision](10-questions-revision.md)            | Vérifier que tout est compris                         |

&#x20;

***

&#x20;

## <mark style="color:purple;">03</mark> · Préparer ton terminal

&#x20;

{% tabs %}
{% tab title="Linux" %}
Rien à faire : ouvre simplement un **terminal** (<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>T</kbd> sur la plupart des distributions).
{% endtab %}

{% tab title="macOS" %}
Ouvre l'application **Terminal**. macOS est un Unix (famille BSD) : presque toutes les commandes sont identiques.
{% endtab %}

{% tab title="Windows" %}
Installe **WSL** (Windows Subsystem for Linux), qui fait tourner un vrai Linux dans Windows :

```powershell
wsl --install
```

[Guide d'installation de WSL](https://learn.microsoft.com/fr-fr/windows/wsl/install)
{% endtab %}

{% tab title="Docker" %}
Lance un shell zsh jetable dans un conteneur :

```bash
docker run -it ohmyzsh/zsh
```

Idéal pour expérimenter sans risquer de casser ta machine. Voir le chapitre [Docker](../docker/README.md).
{% endtab %}
{% endtabs %}

&#x20;

{% hint style="success" %}
**Conseil** — tape toutes les commandes de ce chapitre toi-même. La ligne de commande s'apprend avec les doigts, pas avec les yeux. Prévois ~30 minutes d'essais libres.
{% endhint %}

&#x20;

***

&#x20;

<details>

<summary>Références</summary>

&#x20;

* _Modern Operating Systems_, Andrew S. Tanenbaum & Herbert Bos
* [Introduction to Bash and Bash scripting — GeeksforGeeks](https://www.geeksforgeeks.org/bash-scripting-introduction-to-bash-and-bash-scripting/)
* [Basic shell commands in Linux — GeeksforGeeks](https://www.geeksforgeeks.org/linux-unix/basic-shell-commands-in-linux/)
* [Linux shells — phoenixNAP](https://phoenixnap.com/kb/linux-shells)
* [Linux user groups and permissions guide — daily.dev](https://daily.dev/blog/linux-user-groups-and-permissions-guide/)
* [How do Zsh configuration files work — freeCodeCamp](https://www.freecodecamp.org/news/how-do-zsh-configuration-files-work/)
* [History of Unix & Linux — FrontPageLinux](https://frontpagelinux.com/articles/guide-through-history-of-unix-linux-everything-you-need-to-know/)

&#x20;

_Source : cours « IT Methodology — Shell / Command Line », P. Rétornaz & A. Jungo, HEIA-FR._

&#x20;

</details>
