---
description: Les principaux systèmes de fichiers Linux, sur disque et en mémoire.
---

# 8. Systèmes de fichiers

## Qu'est-ce qu'un système de fichiers ?

Un disque dur, c'est au fond une immense suite de cases vides. Le **système de fichiers** (_filesystem_) est la **méthode d'organisation** qui permet de savoir où est stocké chaque fichier, son nom, sa taille, ses permissions, sa date…

### 📚 L'analogie de la bibliothèque

Le disque, ce sont les **étagères**. Le système de fichiers, c'est le **système de classement** de la bibliothèque : le catalogue, les cotes sur les livres, les règles de rangement. Deux bibliothèques peuvent avoir les mêmes étagères mais des systèmes de classement très différents, plus ou moins rapides ou robustes.

## Systèmes de fichiers sur disque

| Système | Points forts | Usage typique |
| --- | --- | --- |
| **ext4** | Mature, très fiable, **journalisé** | Choix par défaut de la plupart des distributions |
| **XFS** | Très gros fichiers et volumes, excellentes performances en parallèle | Serveurs d'entreprise ; plus complexe à gérer |
| **Btrfs** | **Copy-on-Write**, **snapshots** instantanés, RAID intégré, compression transparente | Système de fichiers « nouvelle génération » (par défaut sur Fedora) |
| **F2FS** | Conçu pour la mémoire **flash** (SSD, NVMe, cartes SD) | Maximiser la durée de vie et les performances des SSD, smartphones |

### La journalisation (ext4)

Un système de fichiers **journalisé** note dans un « journal » les modifications **avant** de les faire. En cas de coupure de courant au milieu d'une écriture, il peut **reprendre ou annuler proprement**, au lieu de laisser des fichiers corrompus.

> 📝 C'est comme un comptable qui note chaque opération dans son cahier **avant** de toucher à la caisse : en cas d'interruption, il sait exactement où il en était.

### Copy-on-Write et snapshots (Btrfs)

Avec le **Copy-on-Write** (CoW), un fichier modifié n'est **jamais écrasé sur place** : la nouvelle version est écrite ailleurs, puis on met à jour le pointeur. Conséquence : on peut créer un **snapshot** (une photo du système à un instant T) **instantanément** et sans copier les données.

> 📸 Utile avant une mise à jour risquée : en cas de problème, on revient au snapshot.

## Systèmes de fichiers en mémoire (virtuels)

Certains systèmes de fichiers n'existent **pas sur le disque** : ils vivent en RAM ou sont générés à la volée par le noyau.

| Système | Monté sur | Rôle |
| --- | --- | --- |
| **tmpfs** | `/tmp`, `/run`… | Stocke des fichiers **en RAM** : très rapide, mais **effacé au redémarrage** |
| **procfs** | `/proc` | Fenêtre sur les **processus** et le noyau, créée à la volée (ex. stats CPU, mémoire) |
| **sysfs** | `/sys` | Vue structurée du **matériel**, des pilotes et de la configuration système |

```bash
cat /proc/cpuinfo        # infos sur le processeur
cat /proc/meminfo        # infos sur la mémoire
ls /proc                 # un dossier numéroté par processus en cours !
cat /proc/version        # version du noyau
ls /sys/class/net        # les interfaces réseau
```

{% hint style="info" %}
Ces fichiers ne prennent **aucune place sur le disque** : quand tu lis `/proc/cpuinfo`, c'est le noyau qui génère le texte **au moment où tu le lis**. C'est l'illustration parfaite de « **tout est fichier** ».
{% endhint %}

{% hint style="success" %}
**Lien avec Docker :** les montages `tmpfs` de Docker (voir [Volumes Docker](../docker/06-volumes.md)) utilisent exactement ce mécanisme pour stocker des données en RAM.
{% endhint %}

## Voir les systèmes de fichiers montés

```bash
df -hT          # espace disque et type de système de fichiers de chaque montage
lsblk -f        # disques, partitions et leurs systèmes de fichiers
mount           # tous les systèmes de fichiers montés
```

```
$ df -hT
Filesystem     Type   Size  Used Avail Use% Mounted on
/dev/nvme0n1p3 btrfs  475G  120G  353G  26% /
tmpfs          tmpfs   16G  4.0M   16G   1% /tmp
/dev/nvme0n1p1 vfat   599M   20M  579M   4% /boot/efi
```

## En résumé

| Type | Exemples | À retenir |
| --- | --- | --- |
| Sur disque | ext4, XFS, Btrfs, F2FS | ext4 = fiable par défaut, Btrfs = snapshots, F2FS = SSD |
| En mémoire | tmpfs | Rapide, mais perdu au redémarrage |
| Virtuels | procfs (`/proc`), sysfs (`/sys`) | Générés par le noyau, exposent processus et matériel |
