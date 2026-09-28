---
description: Comment Linux range ses fichiers — la racine /, le rôle de chaque dossier, et les chemins.
---

# 2. L'arborescence Linux

## Un seul arbre

Sous Windows, chaque disque a sa lettre : `C:\`, `D:\`… Sous Linux, il n'y a **qu'un seul arbre**, qui part d'une **racine** unique notée **`/`** (_slash_, « root »). Tout est rangé en dessous, même les autres disques et les clés USB.

### 🌳 L'analogie de l'arbre généalogique… à l'envers

Imagine un arbre **la tête en bas** : la racine `/` est en haut, et les branches (les dossiers) descendent vers les feuilles (les fichiers).

```
/                               ◄── la racine
├── bin/
├── boot/
├── dev/
├── etc/
│   ├── hostname
│   └── passwd
├── home/
│   ├── alice/                  ◄── le dossier personnel d'alice
│   │   ├── Documents/
│   │   ├── Downloads/
│   │   └── Desktop/
│   └── bob/
├── proc/
├── root/                       ◄── le dossier personnel de l'administrateur
├── tmp/
├── usr/
│   ├── bin/
│   ├── lib/
│   └── local/
└── var/
    └── log/
```

## Le rôle de chaque dossier

Cette organisation suit une norme : le **FHS** (_Filesystem Hierarchy Standard_). Une fois que tu la connais, tu t'y retrouves sur **n'importe quel** système Linux.

| Dossier | Contenu | Pour retenir |
| --- | --- | --- |
| `/` | La **racine** : contient tout le reste | Le tronc de l'arbre |
| `/bin` | **Binaires essentiels** pour tous les utilisateurs (`ls`, `cp`, `cat`…) | _bin_ = binaries |
| `/sbin` | Binaires d'**administration système** (`reboot`, `fdisk`…) | _s_ = system |
| `/etc` | Fichiers de **configuration** du système | « Et cetera », le tiroir des réglages |
| `/home` | Les **dossiers personnels** des utilisateurs (`/home/alice`) | La maison de chacun |
| `/root` | Le dossier personnel de l'**administrateur** (root) | Pas dans `/home`, pour rester accessible même en cas de problème |
| `/usr` | **Programmes et fichiers des utilisateurs** installés : `/usr/bin`, `/usr/lib`, `/usr/share` | _Unix System Resources_ |
| `/usr/local` | Logiciels **installés à la main** par l'administrateur | Ce que tu ajoutes toi-même |
| `/lib` | **Bibliothèques partagées** essentielles (l'équivalent des `.dll` Windows) | _lib_ = libraries |
| `/dev` | Fichiers représentant les **périphériques** (`/dev/sda` = disque, `/dev/null`…) | _dev_ = devices |
| `/proc` | Fichiers **virtuels** sur les **processus** et le noyau | Généré à la volée par le noyau |
| `/sys` | Fichiers **virtuels** sur le **matériel** et les pilotes | Comme `/proc`, pour le matériel |
| `/tmp` | **Fichiers temporaires**, souvent vidés au redémarrage | Le brouillon |
| `/var` | **Données variables** qui grossissent : logs (`/var/log`), caches, bases de données | Ce qui change tout le temps |
| `/boot` | Fichiers de **démarrage** : le noyau (`vmlinuz`), le chargeur | Ce qu'il faut pour booter |
| `/opt` | Logiciels **optionnels** tiers (ex. Google Chrome) | _opt_ = optional |
| `/mnt`, `/media` | Points de **montage** : disques, clés USB | Où « branchent » les autres disques |

{% hint style="info" %}
Sur les distributions récentes, `/bin`, `/sbin` et `/lib` sont souvent de simples **raccourcis** vers `/usr/bin`, `/usr/sbin` et `/usr/lib`. La séparation est historique : à l'époque, `/usr` était parfois sur un autre disque.
{% endhint %}

### 🏠 L'analogie de la maison

| Pièce | Dossier |
| --- | --- |
| La boîte à outils commune | `/bin`, `/usr/bin` |
| Le local technique (accès réservé) | `/sbin` |
| Le tableau des réglages (thermostat, alarme…) | `/etc` |
| Les chambres de chacun | `/home/alice`, `/home/bob` |
| La chambre du propriétaire | `/root` |
| La table de brouillon, nettoyée chaque soir | `/tmp` |
| Le journal de bord de la maison | `/var/log` |
| Les prises murales (brancher des appareils) | `/dev`, `/mnt` |

## « Tout est fichier »

Une idée fondamentale d'Unix : **tout est représenté comme un fichier**. Les documents, bien sûr, mais aussi les **dossiers**, les **périphériques** (`/dev/sda` pour un disque), et même des informations sur le système (`/proc/cpuinfo`).

```bash
cat /proc/cpuinfo     # infos sur le processeur… lues comme un fichier texte !
cat /etc/os-release   # quelle distribution Linux ?
```

## Les dossiers spéciaux

| Symbole | Signification | Exemple |
| --- | --- | --- |
| `/` | La **racine** | `cd /` |
| `~` | Ton **dossier personnel** (_home_) : `/home/ton_nom` | `cd ~` |
| `.` | Le **dossier courant** | `./script.sh` |
| `..` | Le **dossier parent** (un niveau au-dessus) | `cd ..` |
| `-` | Le **dossier précédent** (avec `cd`) | `cd -` |

```bash
cd /          # aller à la racine
cd ~          # aller dans son dossier personnel
cd            # idem : cd sans argument ramène au home
echo $HOME    # affiche le chemin du home, ex. /home/alice
```

## Chemins absolus et relatifs

Pour désigner un fichier, il y a deux façons :

{% tabs %}
{% tab title="Chemin absolu" %}
Commence **toujours par `/`**. Il part de la racine et fonctionne **peu importe où tu te trouves**.

```bash
cat /home/alice/Documents/notes.txt
```

C'est comme une **adresse postale complète** : « Rue de la Gare 12, 1700 Fribourg, Suisse ».
{% endtab %}

{% tab title="Chemin relatif" %}
Ne commence **pas** par `/`. Il part du **dossier où tu te trouves**.

```bash
# si je suis dans /home/alice
cat Documents/notes.txt
cat ./Documents/notes.txt       # identique
cat ../bob/todo.txt             # remonte d'un niveau puis va dans bob
```

C'est comme une **indication de passant** : « deuxième rue à gauche ». Ça ne marche que si l'on sait d'où l'on part.
{% endtab %}
{% endtabs %}

### Exemple guidé

```
/home/
├── alice/          ◄── je suis ici
│   └── Documents/
│       └── notes.txt
└── bob/
    └── todo.txt
```

| Je veux atteindre | Chemin absolu | Chemin relatif (depuis `/home/alice`) |
| --- | --- | --- |
| `notes.txt` | `/home/alice/Documents/notes.txt` | `Documents/notes.txt` |
| `todo.txt` | `/home/bob/todo.txt` | `../bob/todo.txt` |
| la racine | `/` | `../..` |

{% hint style="warning" %}
Linux est **sensible à la casse** : `Documents`, `documents` et `DOCUMENTS` sont trois noms **différents**. Et évite les espaces dans les noms de fichiers : ils compliquent la vie en ligne de commande.
{% endhint %}

## En résumé

* Un **seul arbre**, qui part de la racine **`/`**.
* Chaque dossier a un rôle défini par le **FHS** : `/etc` = config, `/home` = utilisateurs, `/var/log` = logs, `/tmp` = temporaire…
* **Tout est fichier**, même les périphériques.
* `~` = home, `.` = ici, `..` = parent.
* Chemin **absolu** = commence par `/` ; chemin **relatif** = part du dossier courant.
