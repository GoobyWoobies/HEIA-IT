---
description: Pourquoi Docker existe et en quoi un conteneur diffère d'une machine virtuelle.
---

# 1. Introduction à Docker

## Le problème de départ

Imagine : tu développes une application web sur ton laptop. Elle utilise Node.js 22, PostgreSQL 16 et quelques librairies. Tout fonctionne. Tu l'envoies à un collègue… et chez lui, **rien ne marche** :

* il a Node.js 18 au lieu de 22 ;
* PostgreSQL n'est pas installé ;
* il est sous Windows, toi sous Linux ;
* une variable d'environnement manque.

C'est le fameux **« ça marche sur ma machine »**. Le problème ne vient pas du code, mais de **l'environnement** dans lequel il tourne.

## La solution : Docker

> **Docker** est une plateforme ouverte pour **développer, livrer et exécuter** des applications. Il permet de **séparer l'application de l'infrastructure** pour livrer du logiciel rapidement.

L'idée est simple : au lieu d'envoyer seulement ton code, tu envoies **ton code + tout ce dont il a besoin pour tourner** (runtime, librairies, configuration), empaquetés dans une boîte standard. Cette boîte s'appelle un **conteneur**.

### 🚢 L'analogie du conteneur maritime

Avant les années 1950, charger un bateau était un cauchemar : des sacs de café, des tonneaux, des caisses de toutes tailles… Chaque marchandise demandait une manutention différente.

Puis on a inventé le **conteneur maritime standard** : une boîte métallique aux dimensions fixes. Peu importe ce qu'il y a dedans (voitures, bananes, ordinateurs), **tous les ports, grues, bateaux et camions du monde savent le manipuler de la même façon**.

Docker fait exactement la même chose pour les logiciels :

| Monde maritime | Monde Docker |
| --- | --- |
| Marchandise | Ton application + ses dépendances |
| Conteneur standard | Conteneur Docker |
| Bateau, train, camion | Ton laptop, un serveur, le cloud |
| Grue du port | Le moteur Docker |

Ce n'est pas un hasard si le logo de Docker est une baleine qui transporte des conteneurs 🐳.

{% hint style="success" %}
**À retenir :** un conteneur Docker tourne **de la même manière partout** où Docker est installé. L'environnement voyage avec l'application.
{% endhint %}

## Conteneurs vs machines virtuelles

Avant Docker, pour isoler une application, on utilisait des **machines virtuelles (VM)**. Les deux isolent, mais pas au même niveau.

```
      MACHINES VIRTUELLES                      CONTENEURS

┌────────┐┌────────┐┌────────┐
│ App A  ││ App A' ││ App B  │
├────────┤├────────┤├────────┤        ┌────────┐┌────────┐┌────────┐
│Bins/Lib││Bins/Lib││Bins/Lib│        │ App A  ││ App A' ││ App B  │
├────────┤├────────┤├────────┤        ├────────┴┴────────┤├────────┤
│Guest OS││Guest OS││Guest OS│        │    Bins/Libs     ││Bins/Lib│
└────────┘└────────┘└────────┘        └──────────────────┘└────────┘
┌────────────────────────────┐        ┌────────────────────────────┐
│        Hyperviseur         │        │       Docker Engine        │
├────────────────────────────┤        ├────────────────────────────┤
│          OS hôte           │        │          OS hôte           │
├────────────────────────────┤        ├────────────────────────────┤
│          Serveur           │        │          Serveur           │
└────────────────────────────┘        └────────────────────────────┘

 Chaque VM embarque un OS complet      Les conteneurs partagent le
 → lourd (Go, démarrage en minutes)    noyau de l'OS hôte → léger
                                       (Mo, démarrage en secondes)
```

### 🏠 L'analogie du logement

* Une **VM**, c'est une **maison individuelle** : chacune a ses propres fondations, sa plomberie, son électricité. Très isolé, mais coûteux à construire.
* Un **conteneur**, c'est un **appartement dans un immeuble** : chacun a sa porte fermée à clé et son intérieur, mais tous partagent les fondations et les canalisations (le noyau de l'OS). Beaucoup plus rapide et économique.

| Critère | Machine virtuelle | Conteneur |
| --- | --- | --- |
| Contient | Un OS complet | Seulement l'app et ses dépendances |
| Taille | Plusieurs Go | Quelques Mo à centaines de Mo |
| Démarrage | Minutes | Secondes (voire moins) |
| Isolation | Très forte (matériel virtualisé) | Forte (processus isolés, noyau partagé) |
| Nombre sur une machine | Quelques-unes | Des dizaines, voire des centaines |

{% hint style="warning" %}
**Attention :** comme les conteneurs partagent le noyau de l'hôte, un conteneur Linux a besoin d'un noyau Linux. Sur Windows et macOS, Docker Desktop fait tourner discrètement une petite VM Linux pour héberger les conteneurs.
{% endhint %}

## Conteneur ≠ Docker

Docker est l'outil le plus populaire pour créer et gérer des conteneurs, **mais ce n'est pas le seul**. Le concept de conteneur est un standard (OCI — _Open Container Initiative_).

| Outil | Particularité |
| --- | --- |
| **Docker** | Le plus répandu, très simple d'utilisation |
| **Podman** | Alternative sans daemon, fonctionne sans droits root, commandes compatibles Docker |
| **containerd** | Le moteur bas niveau utilisé par Docker lui-même et par Kubernetes |

{% hint style="info" %}
Une image construite avec Docker peut être exécutée par Podman ou containerd, et inversement : elles suivent toutes le standard OCI.
{% endhint %}

## En résumé

* Docker **empaquette une application avec son environnement** dans un conteneur.
* Un conteneur **tourne pareil partout** : fini le « ça marche sur ma machine ».
* Les conteneurs sont **plus légers que les VM** car ils partagent le noyau de l'OS hôte.
* Docker n'est qu'**un outil parmi d'autres** pour gérer des conteneurs.
