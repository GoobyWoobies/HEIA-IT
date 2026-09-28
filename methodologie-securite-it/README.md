---
description: Les outils du quotidien d'un informaticien et les bases de la sécurité IT.
icon: shield-halved
cover: https://placehold.co/1600x500/0f172a/4ade80?text=M%C3%A9thodologie+et+S%C3%A9curit%C3%A9+IT
coverY: 0
---

# Méthodologie et Sécurité IT

<mark style="color:blue;">**Les outils que tout informaticien utilise chaque jour, et comment s'en servir en sécurité.**</mark>

&#x20;

{% hint style="info" %}
**En bref**

On commence par **Linux et la ligne de commande**, puis on apprend à emballer et lancer des applications avec **Docker**. La sécurité est présente partout : permissions, moindre privilège, secrets hors des images…
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Les chapitres

&#x20;

```mermaid
flowchart LR
    L["🐧 Linux & Shell<br/>le terminal, les fichiers,<br/>les permissions"] --> D["🐳 Docker<br/>images, conteneurs,<br/>volumes, Compose"]
    D --> N["➕ Prochains cours…"]

    style L fill:#dcfce7,stroke:#22c55e,color:#14532d
    style D fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style N fill:#f8fafc,stroke:#94a3b8,color:#475569
```

&#x20;

{% columns %}
{% column %}
### 🐧 [Linux & Shell](linux/README.md)

Histoire de Linux, arborescence, commandes de base, redirections et pipes, variables d'environnement, permissions, systèmes de fichiers.

&#x20;

<mark style="color:green;">**10 pages · 33 questions**</mark>
{% endcolumn %}

{% column %}
### 🐳 [Docker](docker/README.md)

Conteneurs vs VM, architecture, images et Dockerfile, conteneurs, volumes, maintenance et Docker Compose.

&#x20;

<mark style="color:green;">**10 pages · 24 questions**</mark>
{% endcolumn %}
{% endcolumns %}

&#x20;

{% hint style="success" %}
**Dans quel ordre ?** Commence par **Linux** : Docker s'appuie sur la ligne de commande, les permissions et les systèmes de fichiers.
{% endhint %}
