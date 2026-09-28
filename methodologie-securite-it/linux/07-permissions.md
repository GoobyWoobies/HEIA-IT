---
description: Utilisateurs, groupes, permissions rwx, chmod, chown et le principe du moindre privilège.
---

# 7. Utilisateurs, groupes et permissions

Linux est un système **multi-utilisateur** depuis toujours : plusieurs personnes peuvent utiliser la même machine. Il faut donc décider **qui a le droit de faire quoi** sur chaque fichier.

## Utilisateurs et groupes

* Chaque personne (ou service) a un **compte utilisateur** : `alice`, `bob`, `www-data`…
* Les utilisateurs peuvent appartenir à des **groupes** : `devs`, `docker`, `sudo`… Donner un droit à un groupe, c'est le donner à tous ses membres.
* **`root`** est le **super-administrateur** : il a **tous les droits**, sur tout.

```bash
whoami           # qui suis-je ? → alice
id               # mon identifiant et mes groupes
groups           # la liste de mes groupes
```

### `sudo` : agir en administrateur

Plutôt que de se connecter en `root`, on utilise **`sudo`** (_superuser do_) pour exécuter **une seule commande** avec les droits d'administrateur :

```bash
sudo apt update          # demande ton mot de passe, puis exécute en root
```

{% hint style="warning" %}
`sudo` donne un pouvoir total. Une erreur avec `sudo rm -rf` peut détruire le système. N'utilise `sudo` que quand c'est **vraiment nécessaire**, et relis la commande.
{% endhint %}

## Lire les permissions

La commande `ls -l` affiche les permissions en début de ligne :

```
$ ls -l /home
drwxr-xr-x 2 alice   alice   4.0K Jul 20 12:41 alice
drwxr-xr-x 2 bob     bob     4.0K Jul 20 12:40 bob
```

Décomposons `drwxr-xr-x` :

```
    d   rwx   r-x   r-x
    │   ─┬─   ─┬─   ─┬─
    │    │     │     └── Others : tous les autres utilisateurs
    │    │     └── Group  : les membres du groupe propriétaire
    │    └── User   : le propriétaire du fichier
    └── Type : d = dossier, - = fichier, l = lien
```

Chaque bloc de 3 lettres indique les droits pour une catégorie de personnes :

| Lettre | Droit | Sur un **fichier** | Sur un **dossier** |
| --- | --- | --- | --- |
| `r` | **R**ead (lire) | Lire le contenu | Lister le contenu (`ls`) |
| `w` | **W**rite (écrire) | Modifier le contenu | Créer / supprimer des fichiers dedans |
| `x` | e**X**ecute (exécuter) | Lancer comme programme | **Entrer** dedans (`cd`) |
| `-` | Pas de droit | | |

Donc `drwxr-xr-x` sur `/home/alice` signifie :

* **alice** (propriétaire) : lire, écrire, entrer → `rwx`
* **groupe alice** : lire et entrer, mais pas modifier → `r-x`
* **tous les autres** : lire et entrer, mais pas modifier → `r-x`

### 🏢 L'analogie de l'immeuble de bureaux

Chaque bureau (fichier) a trois types de badges :

* **User** : le badge du **locataire** du bureau.
* **Group** : le badge de **son équipe**.
* **Others** : le badge **visiteur**.

Et chaque badge ouvre (ou non) trois choses : **lire** les dossiers sur le bureau (`r`), **modifier** ce qui s'y trouve (`w`), **utiliser** les machines (`x`).

## La notation octale (les chiffres)

On représente souvent les permissions par **trois chiffres**. Chaque droit vaut un nombre, et on **additionne** :

| Droit | Valeur |
| --- | --- |
| `r` | **4** |
| `w` | **2** |
| `x` | **1** |
| `-` | 0 |

```
   rwx  =  4 + 2 + 1  =  7
   r-x  =  4 + 0 + 1  =  5
   r--  =  4 + 0 + 0  =  4
   rw-  =  4 + 2 + 0  =  6

   rwxr-xr-x  →  7 5 5  →  755
   rw-r--r--  →  6 4 4  →  644
```

### Les combinaisons courantes

| Octal | Symbolique | Usage typique |
| --- | --- | --- |
| `755` | `rwxr-xr-x` | Dossiers, scripts et programmes |
| `644` | `rw-r--r--` | Fichiers normaux (documents, config) |
| `700` | `rwx------` | Dossier privé |
| `600` | `rw-------` | Fichier privé : **clés SSH**, mots de passe |
| `777` | `rwxrwxrwx` | ⚠️ Tout le monde peut tout faire — **à éviter** |

## Modifier les permissions : `chmod`

`chmod` (_change mode_) s'utilise de deux façons :

{% tabs %}
{% tab title="Avec des chiffres" %}
```bash
chmod 755 script.sh      # rwxr-xr-x
chmod 644 notes.txt      # rw-r--r--
chmod 600 ~/.ssh/id_rsa  # rw------- (clé privée)
chmod -R 755 dossier/    # récursif : dossier et tout son contenu
```
{% endtab %}

{% tab title="Avec des lettres" %}
Format : **qui** (`u` user, `g` group, `o` others, `a` all) + **action** (`+` ajouter, `-` retirer, `=` fixer) + **droit** (`r`, `w`, `x`).

```bash
chmod u+x script.sh      # ajoute l'exécution au propriétaire
chmod g-w notes.txt      # retire l'écriture au groupe
chmod o-rwx secret.txt   # retire tous les droits aux autres
chmod a+r public.txt     # tout le monde peut lire
chmod u=rw,go=r doc.txt  # équivaut à 644
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Cas classique :** tu écris un script `deploy.sh` et `./deploy.sh` répond `Permission denied`. Il manque simplement le droit d'exécution : `chmod u+x deploy.sh`.
{% endhint %}

## Changer le propriétaire : `chown`

`chown` (_change owner_) change l'utilisateur et/ou le groupe propriétaire. Il faut généralement `sudo`.

```bash
sudo chown bob fichier.txt            # propriétaire → bob (le groupe ne change pas)
sudo chown bob:devs fichier.txt       # propriétaire → bob ET groupe → devs
sudo chown :devs fichier.txt          # seulement le groupe → devs
sudo chown -R alice:alice dossier/    # récursif
```

{% hint style="warning" %}
**Piège fréquent :** `chown user fichier` ne change **que** le propriétaire. Pour changer aussi le groupe, il faut `chown user:group fichier`.
{% endhint %}

## Gérer les utilisateurs et groupes

| Commande | Rôle | Exemple |
| --- | --- | --- |
| `useradd` | Créer un utilisateur | `sudo useradd -m alice` (`-m` crée son home) |
| `passwd` | Définir un mot de passe | `sudo passwd alice` |
| `groupadd` | Créer un groupe | `sudo groupadd devs` |
| `usermod -aG` | Ajouter un utilisateur à un groupe | `sudo usermod -aG devs alice` |
| `userdel` | Supprimer un utilisateur | `sudo userdel -r alice` |
| `chmod` | Changer les permissions | `chmod 644 fichier` |
| `chown` | Changer le propriétaire | `sudo chown alice:devs fichier` |

{% hint style="warning" %}
Dans `usermod -aG`, le **`-a`** (_append_) est essentiel : sans lui, l'utilisateur est **retiré de tous ses autres groupes** !
{% endhint %}

{% hint style="info" %}
Après avoir ajouté un utilisateur à un groupe, le changement ne prend effet qu'à sa **prochaine connexion**. Il faut se déconnecter / reconnecter (ou ouvrir un nouveau shell avec `newgrp devs`).
{% endhint %}

📖 Exercice : [Linux user groups and permissions guide — daily.dev](https://daily.dev/blog/linux-user-groups-and-permissions-guide/)

## 🔒 Le principe du moindre privilège

> **Principe du moindre privilège** (_Principle of Least Privilege_, PoLP) : donner à chaque utilisateur **uniquement les accès dont il a besoin pour faire son travail. Rien de plus.**

C'est l'un des piliers de la sécurité informatique. Si un compte est compromis, les dégâts sont limités à ce que ce compte pouvait faire.

### 🔑 L'analogie de l'hôtel

La carte d'un client d'hôtel ouvre **sa** chambre, la salle de sport et l'ascenseur. Pas les autres chambres, pas la cuisine, pas le coffre. Si le client perd sa carte, le voleur ne peut pas vider l'hôtel.

**Applique-le partout :**

* 🐳 **Images Docker** : ne pas faire tourner l'application en `root` dans le conteneur
* 🗄️ **Utilisateurs de base de données** : l'application n'a pas besoin des droits d'administration de la base
* 👤 **Utilisateurs de tes applications** : un simple utilisateur ne doit pas accéder au panneau admin
* 🔑 **Chaque token d'API** que tu génères : lecture seule si l'écriture n'est pas nécessaire
* ✅ Sérieusement : **partout**

## Les erreurs courantes

| Erreur | Pourquoi c'est un problème |
| --- | --- |
| `chmod 777` « pour que ça marche » | N'importe qui peut lire, modifier et exécuter le fichier. On masque le vrai problème en ouvrant une faille |
| `chmod -R` sur le mauvais chemin | Change les droits de milliers de fichiers d'un coup, parfois de fichiers système. Difficile à annuler |
| Mettre `x` sur des fichiers de données | Un `.txt` ou `.csv` n'a pas à être exécutable ; c'est une porte ouverte inutile |
| Compter **uniquement** sur les permissions | La sécurité se fait en **couches** : permissions, pare-feu, chiffrement, mises à jour… |
| Confondre `chown user file` et `chown user:group file` | Le premier ne change pas le groupe |
| Oublier que les changements de groupe demandent une **nouvelle session** | « J'ai ajouté alice au groupe docker mais ça ne marche pas » → elle doit se reconnecter |

## En résumé

* Chaque fichier a un **propriétaire**, un **groupe** et des droits pour **User / Group / Others**.
* `r` = 4, `w` = 2, `x` = 1 → `755` pour les dossiers et scripts, `644` pour les fichiers, `600` pour les secrets.
* `chmod` change les droits, `chown` change le propriétaire.
* **Moindre privilège** : ne donner que le strict nécessaire. **Jamais de `chmod 777`**.
