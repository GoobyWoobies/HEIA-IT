---
description: D'où vient Linux, ce qu'est le noyau, et le rôle du shell.
---

# 1. Introduction : Linux et le shell

## Un peu d'histoire

### Unix, l'ancêtre (1969)

Tout commence en **1969** aux **Bell Labs** (AT&T), où Ken Thompson et Dennis Ritchie créent **Unix**. Sa philosophie a traversé les décennies :

> **« Écrire des programmes qui font une seule chose, et qui la font bien. Écrire des programmes qui travaillent ensemble. »**

C'est exactement ce que tu feras en enchaînant `ls`, `grep`, `sort` et `wc` avec des pipes.

### GNU : le logiciel libre (1983–1985)

* En **1983**, **Richard Stallman** lance le **projet GNU** (_GNU's Not Unix_) : recréer un système de type Unix **entièrement libre**.
* En **1985**, il fonde la **Free Software Foundation (FSF)**.
* La FSF écrit des outils essentiels : l'éditeur **Emacs**, le compilateur **gcc**, **make**, le shell **bash**, les commandes `ls`, `cp`, `cat`…
* Mais il manque la pièce centrale : **le noyau** (_kernel_).

### Linux : le noyau manquant (1991)

* En **1991**, **Linus Torvalds**, étudiant finlandais, écrit la première version du **noyau Linux** et l'associe aux outils GNU existants.
* Résultat : un système d'exploitation complet et libre. C'est pour ça que la FSF préfère parler de **GNU/Linux**.
* Linus Torvalds maintient toujours le noyau aujourd'hui. Son code source est sur [kernel.org](https://www.kernel.org).

```
 1970          1980          1990          2000          2010
  │             │             │             │             │
  ├─ Unix (Bell Labs) ──────────────────────────────────────►  Solaris, AIX, HP-UX…
  │     │
  │     └─ BSD (Berkeley) ──┬──► FreeBSD, NetBSD, OpenBSD
  │                         └──► NeXTSTEP ──► macOS
  │
  │             ├─ GNU (Stallman, 1983) ──┐
  │             │                          ├──► GNU/Linux (1991) ──► Ubuntu, Debian, Fedora…
  │             │    Linux (Torvalds) ─────┘
  │             │
  │             └─ Minix (Tanenbaum) — a inspiré Linus
```

{% hint style="info" %}
**macOS** descend de BSD, donc d'Unix. C'est pour ça que son terminal ressemble tant à celui de Linux. **Android**, lui, utilise le noyau Linux.
{% endhint %}

### Les distributions

Le noyau seul ne suffit pas pour utiliser un ordinateur. Une **distribution** assemble le noyau Linux, les outils GNU, un gestionnaire de paquets, un environnement graphique, etc.

| Distribution | Particularité |
| --- | --- |
| **Ubuntu** | La plus populaire pour débuter, basée sur Debian |
| **Debian** | Très stable, très répandue sur les serveurs |
| **Fedora** | Récente, sponsorisée par Red Hat |
| **Arch Linux** | Minimaliste, tu construis tout toi-même |
| **Alpine** | Ultra-légère (~5 Mo), très utilisée dans les images Docker |

## Les couches d'un système

Un système d'exploitation est organisé en **couches** :

```
┌─────────────────────────────────────────┐
│              ESPACE UTILISATEUR         │  ◄── tes programmes : navigateur,
│  ┌───────────┐                          │      éditeur, ls, grep…
│  │   SHELL   │                          │
├──┴───────────┴──────────────────────────┤
│                 NOYAU (kernel)          │  ◄── gère la mémoire, les processus,
│                                         │      les fichiers, les périphériques
├─────────────────────────────────────────┤
│                 MATÉRIEL                │  ◄── CPU, RAM, disque, réseau
└─────────────────────────────────────────┘
```

* Le **matériel** : le processeur, la mémoire, le disque…
* Le **noyau** : le chef d'orchestre. Il est le **seul** à parler directement au matériel.
* L'**espace utilisateur** : tous les programmes que tu utilises. Ils n'ont **pas le droit** de toucher le matériel directement : ils doivent **demander au noyau**.

## Qu'est-ce qu'un shell ?

> Un **shell** est un **interpréteur de commandes** : il lit ce que tu tapes, le comprend, et demande au système de l'exécuter.

Le mot _shell_ veut dire **coquille** : c'est la couche la plus externe, celle qui enveloppe le noyau et avec laquelle tu interagis.

### 🍽️ L'analogie du restaurant

| Restaurant | Ordinateur |
| --- | --- |
| Toi, le client | L'utilisateur |
| Le serveur qui prend ta commande | Le **shell** |
| La cuisine | Le **noyau** |
| Les fourneaux, frigos, ustensiles | Le **matériel** |

Tu ne vas jamais en cuisine allumer les fourneaux toi-même. Tu dis au serveur ce que tu veux, il transmet à la cuisine, et on te ramène le plat.

### Terminal ≠ shell

On confond souvent les deux :

* Le **terminal** est la **fenêtre** (le programme qui affiche du texte et capte ton clavier) : GNOME Terminal, iTerm2, Windows Terminal…
* Le **shell** est le **programme qui tourne dedans** et interprète tes commandes : bash, zsh…

Le terminal, c'est le téléphone ; le shell, c'est la personne au bout du fil.

## Que se passe-t-il quand je tape `ls` ?

```
  ESPACE UTILISATEUR
      ┌─────────┐
      │  SHELL  │  1. interprète "ls" et lance le programme
      └────┬────┘
      ┌────▼────┐
      │   ls    │◄──────────────┐  4. le résultat revient à ls,
      └────┬────┘               │     qui l'affiche à l'écran
  ─────────┼────────────────────┼─────────────────────────────
  NOYAU    │ 2. appel système   │
           │  (system call)     │
  ─────────┼────────────────────┼─────────────────────────────
  MATÉRIEL ▼ 3. lecture disque ─┘
```

1. Le **shell** lit `ls`, trouve le programme correspondant et le lance.
2. `ls` demande au **noyau** le contenu du dossier, via un **appel système** (_system call_).
3. Le noyau lit l'information sur le **disque**.
4. Le résultat remonte jusqu'à `ls`, qui l'affiche.

{% hint style="info" %}
Les échanges entre l'espace utilisateur et le noyau s'appellent des **appels système** (_system calls_). Pour aller plus loin : Tanenbaum, _Modern Operating Systems_, chapitre 1.6.
{% endhint %}

## Les différents shells

Il existe plusieurs shells, chacun avec ses particularités :

| Shell | Année | Description |
| --- | --- | --- |
| **sh** (Bourne shell) | 1979 | Le premier shell par défaut d'Unix. La référence historique |
| **csh** (C shell) | fin 1970 | Syntaxe inspirée du langage C |
| **tcsh** (TENEX C shell) | début 1980 | Extension de csh |
| **ksh** (KornShell) | début 1980 | Basé sur sh, très utilisé sur les Unix commerciaux |
| **bash** (Bourne Again Shell) | 1989 | Extension de sh par le projet GNU. **Le shell par défaut de la plupart des Linux** |
| **dash** (Debian Almquist) | fin 1990 | Très léger et rapide, utilisé pour les scripts système sur Debian/Ubuntu |
| **zsh** (Z shell) | début 1990 | Extension de sh, très personnalisable. **Shell par défaut de macOS** depuis 2019 |
| **fish** (Friendly Interactive Shell) | milieu 2000 | Axé sur le confort (suggestions, couleurs), mais **pas compatible POSIX** |

{% hint style="success" %}
**POSIX** est une norme qui définit comment un shell doit se comporter. Un script écrit pour `sh` en respectant POSIX fonctionnera dans bash, zsh, ksh ou dash. C'est pour ça que les scripts portables commencent souvent par `#!/bin/sh`.
{% endhint %}

Pour savoir quel shell tu utilises :

```bash
echo $SHELL
```

## En résumé

* **Unix** (1969) → **GNU** (1983, outils libres) + **Linux** (1991, noyau) = **GNU/Linux**.
* Le **noyau** est le seul à parler au matériel ; les programmes lui passent par des **appels système**.
* Le **shell** interprète tes commandes ; le **terminal** est juste la fenêtre qui l'affiche.
* **bash** et **zsh** sont les shells les plus courants.
