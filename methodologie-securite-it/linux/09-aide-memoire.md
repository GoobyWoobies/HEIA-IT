---
description: Les commandes Linux essentielles sur une seule page.
icon: clipboard-list
cover: https://placehold.co/1600x500/0f172a/4ade80?text=Linux+%C2%B7+Aide-m%C3%A9moire
coverY: 0
---

# 9. Aide-mémoire

<mark style="color:blue;">**Toutes les commandes, une seule page.**</mark>

&#x20;

{% hint style="info" %}
**Astuce** — garde cette page ouverte dans un onglet pendant les exercices. Et n'oublie pas : `man <commande>` a toujours la réponse.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Aide et raccourcis

&#x20;

| Action                  | Commande / touche                                        |
| ----------------------- | -------------------------------------------------------- |
| Manuel d'une commande   | `man <commande>`                                         |
| Aide rapide             | `<commande> --help`                                      |
| Autocomplétion          | <kbd>Tab</kbd>                                           |
| Historique              | <kbd>↑</kbd> · `history` · <kbd>Ctrl</kbd>+<kbd>R</kbd>  |
| Interrompre             | <kbd>Ctrl</kbd>+<kbd>C</kbd>                             |
| Effacer l'écran         | `clear` · <kbd>Ctrl</kbd>+<kbd>L</kbd>                   |

&#x20;

## <mark style="color:purple;">02</mark> · Navigation

&#x20;

| Action                  | Commande                     |
| ----------------------- | ---------------------------- |
| Où suis-je ?            | `pwd`                        |
| Lister                  | `ls` · `ls -la` · `ls -lh`   |
| Changer de dossier      | `cd <chemin>`                |
| Aller au home           | `cd` ou `cd ~`               |
| Remonter d'un niveau    | `cd ..`                      |
| Dossier précédent       | `cd -`                       |

&#x20;

## <mark style="color:purple;">03</mark> · Fichiers et dossiers

&#x20;

| Action                  | Commande                                   |
| ----------------------- | ------------------------------------------ |
| Créer un dossier        | `mkdir <nom>` (`-p` pour une hiérarchie)   |
| Créer un fichier vide   | `touch <fichier>`                          |
| Copier                  | `cp <src> <dest>` (`-r` pour un dossier)   |
| Déplacer / renommer     | `mv <src> <dest>`                          |
| Supprimer un fichier    | `rm <fichier>` |
| Supprimer un dossier    | `rm -r <dossier>` |
| Chercher un fichier     | `find . -name "*.txt"`                     |
| Type d'un fichier       | `file <fichier>`                           |
| Taille d'un dossier     | `du -sh <dossier>`                         |
| Espace disque           | `df -h`                                    |

&#x20;

## <mark style="color:purple;">04</mark> · Lire et chercher

&#x20;

| Action                          | Commande                                   |
| ------------------------------- | ------------------------------------------ |
| Afficher un texte / variable    | `echo "texte"` · `echo $VAR`               |
| Afficher un fichier             | `cat <fichier>`                            |
| Parcourir un fichier            | `less <fichier>` (<kbd>q</kbd> = quitter)  |
| Début / fin                     | `head -n 5` · `tail -n 5`                  |
| Suivre en direct                | `tail -f <fichier>`                        |
| Chercher un motif               | `grep <motif> <fichier>`                   |
| Sans casse / numéros / inversé  | `grep -i` · `grep -n` · `grep -v`          |
| Chercher dans un dossier        | `grep -r <motif> <dossier>`                |
| Compter les lignes              | `wc -l <fichier>`                          |
| Trier                           | `sort` (`-r` inversé, `-n` numérique)      |
| Dédoublonner                    | `sort \| uniq` (`-c` pour compter)         |
| Comparer                        | `diff <a> <b>`                             |

&#x20;

## <mark style="color:purple;">05</mark> · Redirections et pipes

&#x20;

| Symbole               | Effet                             |
| --------------------- | --------------------------------- |
| `cmd > f`             | Sortie → fichier (écrase)         |
| `cmd >> f`            | Sortie → fichier (ajoute)         |
| `cmd 2> f`            | Erreurs → fichier                 |
| `cmd > f 2>&1`        | Sortie + erreurs → fichier        |
| `cmd < f`             | Fichier → entrée                  |
| `cmd 2> /dev/null`    | Jeter les erreurs                 |
| `cmd1 \| cmd2`        | Sortie de cmd1 → entrée de cmd2   |

&#x20;

## <mark style="color:purple;">06</mark> · Variables d'environnement

&#x20;

| Action                  | Commande                                  |
| ----------------------- | ----------------------------------------- |
| Toutes les variables    | `printenv` · `env`                        |
| Une variable            | `echo $HOME`                              |
| Définir et exporter     | `export NOM=valeur`                       |
| Ajouter au PATH         | `export PATH=$PATH:/nouveau/dossier`      |
| Où est une commande ?   | `which <commande>`                        |
| Recharger la config     | `source ~/.zshrc`                         |

&#x20;

## <mark style="color:purple;">07</mark> · Utilisateurs et permissions

&#x20;

| Action                   | Commande                                |
| ------------------------ | --------------------------------------- |
| Qui suis-je ?            | `whoami` · `id` · `groups`              |
| Exécuter en admin        | `sudo <commande>`                       |
| Changer les droits       | `chmod 755 <f>` · `chmod u+x <f>`       |
| Changer le propriétaire  | `sudo chown user:group <f>`             |
| Créer un utilisateur     | `sudo useradd -m <user>`                |
| Mot de passe             | `sudo passwd <user>`                    |
| Créer un groupe          | `sudo groupadd <groupe>`                |
| Ajouter à un groupe      | `sudo usermod -aG <groupe> <user>`      |

&#x20;

{% columns %}
{% column %}
**`r` = 4 · `w` = 2 · `x` = 1**
{% endcolumn %}

{% column %}
`755` = `rwxr-xr-x` · `644` = `rw-r--r--` · `600` = `rw-------` · <mark style="color:red;">~~`777`~~</mark>
{% endcolumn %}
{% endcolumns %}

&#x20;

## <mark style="color:purple;">08</mark> · Système

&#x20;

| Action                        | Commande                    |
| ----------------------------- | --------------------------- |
| Processus en cours            | `ps aux`                    |
| Moniteur temps réel           | `top` · `htop`              |
| Arrêter un processus          | `kill <PID>`                |
| Distribution installée        | `cat /etc/os-release`       |
| Version du noyau              | `uname -r`                  |
| Systèmes de fichiers montés   | `df -hT` · `lsblk -f`       |

&#x20;

<mark style="color:green;">**→ Suite :**</mark> [10. Questions de révision](10-questions-revision.md)
