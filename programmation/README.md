---
description: Apprendre à programmer en Java, des premières variables jusqu'à la programmation orientée objet.
icon: code
cover: https://placehold.co/1600x500/0f172a/f472b6?text=Programmation
coverY: 0
---

# Programmation

<mark style="color:blue;">**Écrire du code qui marche, et qui se lit.**</mark>

&#x20;

{% hint style="info" %}
**En bref**

Le cours part des **bases du langage Java** (variables, opérateurs, boucles, méthodes, tableaux), passe à la **programmation orientée objet** (classes, héritage, interfaces), puis aborde des notions **avancées** (généricité, lambdas, enums) et les **bonnes pratiques**.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Le parcours

&#x20;

```mermaid
flowchart LR
    subgraph P1["Partie 1 · Les bases du langage"]
        direction TB
        C1["1. Introduction"] --> C2["2. Expressions"] --> C3["3. Contrôle de flux"] --> C4["4. Méthodes"] --> C5["5. Tableaux"] --> C6["6. Exceptions"] --> C7["7. String et E/S"]
    end
    subgraph P2["Partie 2 · Programmation orientée objet"]
        direction TB
        C8["8. Classes et objets"] --> C9["9. Packages et encapsulation"] --> C10["10. Membres statiques"] --> C11["11. Héritage"] --> C12["12. Abstraites et interfaces"]
    end
    subgraph P3["Partie 3 · Pour aller plus loin"]
        direction TB
        C13["13. Généricité et lambdas"] --> C14["14. Enum et annotations"] --> C15["15. Bonnes pratiques"]
    end
    P1 --> P2 --> P3

    style P1 fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style P2 fill:#ede9fe,stroke:#8b5cf6,color:#4c1d95
    style P3 fill:#fce7f3,stroke:#ec4899,color:#831843
    style C1 fill:#dcfce7,stroke:#22c55e,color:#14532d
    style C2 fill:#dcfce7,stroke:#22c55e,color:#14532d
    style C3 fill:#dcfce7,stroke:#22c55e,color:#14532d
```

&#x20;

***

&#x20;

## <mark style="color:purple;">02</mark> · Les chapitres

&#x20;

| #  | Chapitre                                                                                                    | Statut                                              |
| -- | ----------------------------------------------------------------------------------------------------------- | --------------------------------------------------- |
| 1  | [Introduction et éléments de base](01-introduction-elements-de-base/README.md)                              | <mark style="color:green;">**Rédigé**</mark>        |
| 2  | [Expressions et opérateurs](02-expressions-operateurs/README.md)                                            | <mark style="color:green;">**Rédigé**</mark>        |
| 3  | [Instructions et contrôle de flux](03-instructions-controle-flux/README.md)                                 | <mark style="color:green;">**Rédigé**</mark>        |
| 4  | [Méthodes](04-methodes/README.md)                                                                           | À venir                                             |
| 5  | [Tableaux](05-tableaux/README.md)                                                                           | À venir                                             |
| 6  | [Exceptions](06-exceptions/README.md)                                                                       | À venir                                             |
| 7  | [String et entrées/sorties](07-string-entrees-sorties/README.md)                                            | À venir                                             |
| 8  | [Classes et objets](08-classes-objets/README.md)                                                            | À venir                                             |
| 9  | [Packages, contrôle d'accès, initialiseurs et encapsulation](09-packages-controle-acces-initialiseurs-encapsulation/README.md) | À venir                  |
| 10 | [Membres statiques, initialiseurs et wrappers](10-membres-statiques-initialiseurs-wrappers/README.md)       | À venir                                             |
| 11 | [Héritage et polymorphisme](11-heritage-polymorphisme/README.md)                                            | À venir                                             |
| 12 | [Classes abstraites et interfaces](12-classes-abstraites-interfaces/README.md)                              | À venir                                             |
| 13 | [Généricité et expressions lambda](13-genericite-expressions-lambda/README.md)                              | À venir                                             |
| 14 | [Enum, annotations, RSA et limitations](14-enum-annotations-rsa-limitations/README.md)                      | À venir                                             |
| 15 | [Conventions de codage et bonnes pratiques](15-conventions-codage-bonnes-pratiques/README.md)               | À venir                                             |

&#x20;

***

&#x20;

## <mark style="color:purple;">03</mark> · Comment travailler ce cours

&#x20;

{% stepper %}
{% step %}
### Lire le chapitre

&#x20;

Chaque chapitre suit le même format : un encadré **En bref**, des sections numérotées avec des schémas et des exemples de code, puis un **résumé**.
{% endstep %}

{% step %}
### Taper les exemples

&#x20;

Recopie les exemples de code et exécute-les. Modifie-les pour voir ce qui change.
{% endstep %}

{% step %}
### Faire les exercices sur papier

&#x20;

Chaque chapitre se termine par des **exercices à faire au crayon**, comme à l'examen écrit. Ouvre la solution seulement après avoir écrit ta réponse.
{% endstep %}
{% endstepper %}

&#x20;

{% hint style="success" %}
**Outils** — un **JDK** récent (21 ou plus) et un éditeur comme IntelliJ IDEA ou VS Code. Mais pour les exercices : papier et crayon.
{% endhint %}
