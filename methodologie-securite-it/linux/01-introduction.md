---
description: D'où vient Linux, ce qu'est le noyau, et le rôle du shell.
icon: book-open
cover: https://placehold.co/1600x500/0f172a/4ade80?text=Linux+%C2%B7+Introduction
coverY: 0
---

# 1. Linux et le shell

<mark style="color:blue;">**Un système né d'une idée simple : des petits outils qui travaillent ensemble.**</mark>

&#x20;

{% hint style="info" %}
**En bref**

**Linux** est le noyau d'un système d'exploitation libre, associé aux outils du projet **GNU**. Le **noyau** parle au matériel ; le **shell** est le programme qui te permet de parler au système en tapant des commandes.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Un peu d'histoire

&#x20;

```mermaid
timeline
    title De Unix à Linux
    1969 : Unix naît aux Bell Labs
         : Ken Thompson et Dennis Ritchie
    1977 : BSD à Berkeley
         : ancêtre de macOS
    1983 : Projet GNU
         : Richard Stallman
    1985 : Free Software Foundation
    1987 : Minix
         : Andrew Tanenbaum
    1991 : Noyau Linux
         : Linus Torvalds
```

&#x20;

{% stepper %}
{% step %}
### Unix, l'ancêtre (1969)

&#x20;

Aux **Bell Labs** (AT&T), Ken Thompson et Dennis Ritchie créent **Unix**. Sa philosophie traverse les décennies :

&#x20;

> **« Écrire des programmes qui font une seule chose, et qui la font bien. Écrire des programmes qui travaillent ensemble. »**

&#x20;

C'est exactement ce que tu feras en enchaînant `ls`, `grep` et `wc` avec des pipes.
{% endstep %}

{% step %}
### GNU, le logiciel libre (1983–1985)

&#x20;

**Richard Stallman** lance le **projet GNU** (_GNU's Not Unix_) en 1983 pour recréer un Unix **entièrement libre**, puis fonde la **Free Software Foundation** (FSF) en 1985.

&#x20;

La FSF écrit des outils essentiels : **Emacs**, le compilateur **gcc**, **make**, le shell **bash**, les commandes `ls`, `cp`, `cat`… Mais il manque la pièce centrale : <mark style="color:orange;">**le noyau**</mark>.
{% endstep %}

{% step %}
### Linux, le noyau manquant (1991)

&#x20;

**Linus Torvalds**, étudiant finlandais, écrit la première version du **noyau Linux** et l'associe aux outils GNU. Résultat : un système complet et libre. C'est pour ça que la FSF préfère dire <mark style="color:blue;">**GNU/Linux**</mark>.

&#x20;

Linus maintient toujours le noyau aujourd'hui : son code est sur [kernel.org](https://www.kernel.org).
{% endstep %}
{% endstepper %}

&#x20;

### L'arbre généalogique

&#x20;

```mermaid
flowchart LR
    U["Unix<br/>Bell Labs, 1969"] --> BSD["BSD"]
    U --> COM["Unix commerciaux<br/>Solaris, AIX, HP-UX"]
    BSD --> FBSD["FreeBSD · OpenBSD"]
    BSD --> NX["NeXTSTEP"] --> MAC["macOS"]
    GNU["Outils GNU<br/>1983"] --> GL["GNU/Linux<br/>1991"]
    LIN["Noyau Linux"] --> GL
    MINIX["Minix"] -.->|"a inspiré"| LIN
    GL --> D["Ubuntu · Debian · Fedora…"]
    LIN --> AND["Android"]

    style U fill:#fef3c7,stroke:#f59e0b,color:#78350f
    style GL fill:#dcfce7,stroke:#22c55e,color:#14532d
    style MAC fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style AND fill:#dcfce7,stroke:#22c55e,color:#14532d
```

&#x20;

{% hint style="info" %}
**Le savais-tu ?** macOS descend de BSD, donc d'Unix : c'est pour ça que son terminal ressemble tant à celui de Linux. Et **Android** tourne sur le noyau Linux.
{% endhint %}

&#x20;

### Les distributions

&#x20;

Le noyau seul ne suffit pas. Une **distribution** assemble le noyau, les outils GNU, un gestionnaire de paquets, un bureau graphique…

&#x20;

| Distribution     | Particularité                                                  |
| ---------------- | -------------------------------------------------------------- |
| **Ubuntu**    | La plus populaire pour débuter, basée sur Debian               |
| **Debian**    | Très stable, omniprésente sur les serveurs                     |
| **Fedora**    | Récente, sponsorisée par Red Hat                               |
| **Arch**      | Minimaliste : tu construis tout toi-même                       |
| **Alpine**    | Ultra-légère (~5 Mo), star des images Docker                   |

&#x20;

***

&#x20;

## <mark style="color:purple;">02</mark> · Les couches d'un système

&#x20;

```mermaid
flowchart TB
    subgraph US["Espace utilisateur"]
        direction LR
        SH["Shell"]
        APP["Navigateur · Éditeur · ls · grep…"]
    end
    K["Noyau (kernel)<br/>mémoire · processus · fichiers · périphériques"]
    HW["Matériel<br/>CPU · RAM · disque · réseau"]
    US --> K --> HW

    style US fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style SH fill:#fef3c7,stroke:#f59e0b,color:#78350f
    style K fill:#dcfce7,stroke:#22c55e,color:#14532d
    style HW fill:#fee2e2,stroke:#ef4444,color:#7f1d1d
```

&#x20;

{% columns %}
{% column %}
**Matériel**

Le processeur, la mémoire, le disque…
{% endcolumn %}

{% column %}
**Noyau**

Le chef d'orchestre. Le **seul** à parler directement au matériel.
{% endcolumn %}

{% column %}
**Espace utilisateur**

Tous tes programmes. Ils n'ont **pas le droit** de toucher le matériel : ils **demandent au noyau**.
{% endcolumn %}
{% endcolumns %}

&#x20;

***

&#x20;

## <mark style="color:purple;">03</mark> · Qu'est-ce qu'un shell ?

&#x20;

> Un **shell** est un **interpréteur de commandes** : il lit ce que tu tapes, le comprend, et demande au système de l'exécuter.

&#x20;

_Shell_ veut dire **coquille** : c'est la couche la plus externe, celle qui enveloppe le noyau et avec laquelle tu interagis.

&#x20;

### L'analogie du restaurant

&#x20;

| Restaurant                        | Ordinateur          |
| ------------------------------------ | ---------------------- |
| Toi, le client                       | L'utilisateur          |
| Le serveur qui prend ta commande     | Le **shell**           |
| La cuisine                           | Le **noyau**           |
| Les fourneaux, frigos, ustensiles    | Le **matériel**        |

&#x20;

Tu ne vas jamais en cuisine allumer les fourneaux toi-même. Tu dis au serveur ce que tu veux, il transmet, et on te ramène le plat.

&#x20;

### Terminal ≠ shell

&#x20;

{% columns %}
{% column %}
**Le terminal**

La **fenêtre** qui affiche du texte et capte ton clavier : GNOME Terminal, iTerm2, Windows Terminal…

Le téléphone.
{% endcolumn %}

{% column %}
**Le shell**

Le **programme qui tourne dedans** et interprète tes commandes : bash, zsh…

La personne au bout du fil.
{% endcolumn %}
{% endcolumns %}

&#x20;

<figure><img src="https://placehold.co/1200x300/0f172a/4ade80?text=alice%40laptop%3A~%24+ls+%2F" alt="Un terminal qui exécute ls /"><figcaption><p>Un terminal, dans lequel le shell attend ta prochaine commande après le <code>$</code>.</p></figcaption></figure>

&#x20;

***

&#x20;

## <mark style="color:purple;">04</mark> · Que se passe-t-il quand je tape `ls` ?

&#x20;

```mermaid
sequenceDiagram
    autonumber
    actor U as Toi
    participant S as Shell
    participant L as ls
    participant K asNoyau
    participant D as Disque

    U->>S: ls
    S->>L: Trouve et lance le programme ls
    L->>K: Appel système · « contenu du dossier ? »
    K->>D: Lecture
    D-->>K: Données
    K-->>L: Liste des fichiers
    L-->>U: Affichage à l'écran
```

&#x20;

{% hint style="info" %}
Les échanges entre l'espace utilisateur et le noyau s'appellent des <mark style="color:blue;">**appels système**</mark> (_system calls_). Pour aller plus loin : Tanenbaum, _Modern Operating Systems_, chapitre 1.6.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">05</mark> · Les différents shells

&#x20;

```mermaid
timeline
    title La famille des shells
    1979 : sh · Bourne shell
         : csh · C shell
    1980s : tcsh
          : ksh · KornShell
    1989 : bash · Bourne Again shell
    1990 : zsh · Z shell
    1997 : dash
    2005 : fish
```

&#x20;

| Shell                           | Description                                                                                          |
| ------------------------------- | ---------------------------------------------------------------------------------------------------- |
| **sh** (Bourne shell)           | Le premier shell par défaut d'Unix. La référence historique                                          |
| **csh** / **tcsh**              | Syntaxe inspirée du langage C                                                                        |
| **ksh** (KornShell)             | Basé sur sh, très utilisé sur les Unix commerciaux                                                   |
| **bash** (Bourne Again Shell)   | Extension de sh par GNU. <mark style="color:green;">**Le shell par défaut de la plupart des Linux**</mark> |
| **dash**                        | Très léger et rapide, pour les scripts système sur Debian/Ubuntu                                     |
| **zsh** (Z shell)               | Extension de sh, très personnalisable. <mark style="color:green;">**Par défaut sur macOS**</mark> depuis 2019 |
| **fish**                        | Axé confort (suggestions, couleurs), mais <mark style="color:orange;">**pas compatible POSIX**</mark>  |

&#x20;

{% hint style="success" %}
**POSIX** est une norme qui définit comment un shell doit se comporter. Un script qui la respecte fonctionne dans bash, zsh, ksh ou dash. D'où les scripts qui commencent par `#!/bin/sh`.
{% endhint %}

&#x20;

Quel shell utilises-tu ?

&#x20;

```bash
echo $SHELL
```

&#x20;

***

&#x20;

## <mark style="color:purple;">06</mark> · En résumé

&#x20;

{% hint style="success" %}
* **Unix** (1969) → **GNU** (1983, outils libres) + **Linux** (1991, noyau) = **GNU/Linux**.
* Le **noyau** est le seul à parler au matériel ; les programmes passent par des **appels système**.
* Le **shell** interprète tes commandes ; le **terminal** n'est que la fenêtre.
* **bash** et **zsh** sont les shells les plus courants.
{% endhint %}

&#x20;

<mark style="color:green;">**→ Suite :**</mark> [2. L'arborescence](02-arborescence.md)
