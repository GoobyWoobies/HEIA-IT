---
description: Les commandes Linux essentielles sur une seule page.
---

# 9. Aide-mémoire

## Aide et raccourcis

| Action | Commande / touche |
| --- | --- |
| Manuel d'une commande | `man <commande>` |
| Aide rapide | `<commande> --help` |
| Autocomplétion | <kbd>Tab</kbd> |
| Historique | <kbd>↑</kbd> / `history` / <kbd>Ctrl</kbd>+<kbd>R</kbd> |
| Interrompre | <kbd>Ctrl</kbd>+<kbd>C</kbd> |
| Effacer l'écran | `clear` / <kbd>Ctrl</kbd>+<kbd>L</kbd> |

## Navigation

| Action | Commande |
| --- | --- |
| Où suis-je ? | `pwd` |
| Lister | `ls`, `ls -la`, `ls -lh` |
| Changer de dossier | `cd <chemin>` |
| Aller au home | `cd` ou `cd ~` |
| Remonter d'un niveau | `cd ..` |
| Dossier précédent | `cd -` |

## Fichiers et dossiers

| Action | Commande |
| --- | --- |
| Créer un dossier | `mkdir <nom>` (`-p` pour une hiérarchie) |
| Créer un fichier vide | `touch <fichier>` |
| Copier | `cp <source> <dest>` (`-r` pour un dossier) |
| Déplacer / renommer | `mv <source> <dest>` |
| Supprimer un fichier | `rm <fichier>` ⚠️ |
| Supprimer un dossier | `rm -r <dossier>` ⚠️ |
| Chercher un fichier | `find . -name "*.txt"` |
| Type d'un fichier | `file <fichier>` |
| Taille d'un dossier | `du -sh <dossier>` |
| Espace disque | `df -h` |

## Lire et chercher du texte

| Action | Commande |
| --- | --- |
| Afficher un texte / une variable | `echo "texte"` / `echo $VAR` |
| Afficher un fichier | `cat <fichier>` |
| Parcourir un fichier | `less <fichier>` (<kbd>q</kbd> pour quitter) |
| Premières / dernières lignes | `head -n 5` / `tail -n 5` |
| Suivre un fichier en direct | `tail -f <fichier>` |
| Chercher un motif | `grep <motif> <fichier>` |
| Chercher sans casse / numéros / inversé | `grep -i` / `grep -n` / `grep -v` |
| Chercher dans un dossier | `grep -r <motif> <dossier>` |
| Compter les lignes | `wc -l <fichier>` |
| Trier | `sort` (`-r` inversé, `-n` numérique) |
| Supprimer les doublons | `sort \| uniq` (`-c` pour compter) |
| Comparer deux fichiers | `diff <a> <b>` |

## Redirections et pipes

| Symbole | Effet |
| --- | --- |
| `cmd > f` | Sortie → fichier (écrase) |
| `cmd >> f` | Sortie → fichier (ajoute) |
| `cmd 2> f` | Erreurs → fichier |
| `cmd > f 2>&1` | Sortie + erreurs → fichier |
| `cmd < f` | Fichier → entrée |
| `cmd 2> /dev/null` | Jeter les erreurs |
| `cmd1 \| cmd2` | Sortie de cmd1 → entrée de cmd2 |

## Variables d'environnement

| Action | Commande |
| --- | --- |
| Toutes les variables | `printenv` / `env` |
| Une variable | `echo $HOME` |
| Définir et exporter | `export NOM=valeur` |
| Ajouter au PATH | `export PATH=$PATH:/nouveau/dossier` |
| Où est une commande ? | `which <commande>` |
| Recharger la config | `source ~/.zshrc` |

## Utilisateurs et permissions

| Action | Commande |
| --- | --- |
| Qui suis-je ? | `whoami`, `id`, `groups` |
| Exécuter en admin | `sudo <commande>` |
| Changer les droits | `chmod 755 <f>` / `chmod u+x <f>` |
| Changer le propriétaire | `sudo chown user:group <f>` |
| Créer un utilisateur | `sudo useradd -m <user>` |
| Définir un mot de passe | `sudo passwd <user>` |
| Créer un groupe | `sudo groupadd <groupe>` |
| Ajouter à un groupe | `sudo usermod -aG <groupe> <user>` |

### Permissions en octal

| | r | w | x |
| --- | --- | --- | --- |
| Valeur | 4 | 2 | 1 |

`755` = `rwxr-xr-x` · `644` = `rw-r--r--` · `600` = `rw-------` · ~~`777`~~

## Système

| Action | Commande |
| --- | --- |
| Processus en cours | `ps aux` |
| Moniteur temps réel | `top` / `htop` |
| Arrêter un processus | `kill <PID>` |
| Distribution installée | `cat /etc/os-release` |
| Version du noyau | `uname -r` |
| Systèmes de fichiers montés | `df -hT`, `lsblk -f` |
