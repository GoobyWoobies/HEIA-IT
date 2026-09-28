---
description: Page modèle — le format et le style à reprendre partout dans la doc.
icon: sparkles
cover: https://placehold.co/1600x500/0f172a/38bdf8?text=%E2%9C%A6++Documentation
coverY: 0
---

# Guide de démarrage

<mark style="color:blue;">**Tout ce qu'il faut pour être opérationnel en 5 minutes.**</mark>

&#x20;

Cette page sert de **modèle**. Reprends sa structure, son rythme et ses couleurs pour chaque nouvelle page.

&#x20;

{% hint style="info" %}
**En bref**

On installe, on configure, on lance. Trois étapes, rien de plus.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">01</mark> · Vue d'ensemble

&#x20;

Avant de commencer, voici comment les pièces s'emboîtent.

&#x20;

```mermaid
flowchart LR
    U(["👤 Utilisateur"]) --> A["🖥️ Application"]
    A --> API["⚙️ API"]
    API --> DB[("🗄️ Base de données")]
    API --> C["☁️ Cache"]

    style U fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style A fill:#ede9fe,stroke:#8b5cf6,color:#4c1d95
    style API fill:#fce7f3,stroke:#ec4899,color:#831843
    style DB fill:#dcfce7,stroke:#22c55e,color:#14532d
    style C fill:#fef3c7,stroke:#f59e0b,color:#78350f
```

&#x20;

<figure><img src="https://placehold.co/1200x600/f8fafc/64748b?text=Capture+d%27%C3%A9cran+de+l%27interface" alt="Interface principale"><figcaption><p>L'interface principale, telle qu'elle apparaît au premier lancement.</p></figcaption></figure>

&#x20;

***

&#x20;

## <mark style="color:purple;">02</mark> · Installation

&#x20;

{% stepper %}
{% step %}
### Installer l'outil

&#x20;

```bash
npm install mon-outil
```

&#x20;
{% endstep %}

{% step %}
### Créer la configuration

&#x20;

Ajoute un fichier `config.json` à la racine du projet.

&#x20;

```json
{
  "env": "production",
  "port": 3000
}
```

&#x20;
{% endstep %}

{% step %}
### Lancer

&#x20;

```bash
npm start
```

&#x20;

<mark style="color:green;">**✓ C'est prêt.**</mark> Ouvre `http://localhost:3000`.
{% endstep %}
{% endstepper %}

&#x20;

{% tabs %}
{% tab title="🍎 macOS" %}
```bash
brew install mon-outil
```
{% endtab %}

{% tab title="🪟 Windows" %}
```powershell
winget install mon-outil
```
{% endtab %}

{% tab title="🐧 Linux" %}
```bash
sudo apt install mon-outil
```
{% endtab %}
{% endtabs %}

&#x20;

***

&#x20;

## <mark style="color:purple;">03</mark> · Comment ça marche

&#x20;

Une requête suit toujours le même chemin :

&#x20;

```mermaid
sequenceDiagram
    autonumber
    actor U as 👤 Utilisateur
    participant A as 🖥️ App
    participant API as ⚙️ API
    participant DB as 🗄️ Base

    U->>A: Clique sur « Envoyer »
    A->>API: POST /messages
    API->>DB: Enregistre
    DB-->>API: OK
    API-->>A: 201 Created
    A-->>U: ✅ Message envoyé
```

&#x20;

{% columns %}
{% column %}
<figure><img src="https://placehold.co/600x400/ecfdf5/16a34a?text=%E2%9C%93+Succ%C3%A8s" alt="Écran de succès"><figcaption><p>Ce que l'utilisateur voit quand tout va bien.</p></figcaption></figure>
{% endcolumn %}

{% column %}
<figure><img src="https://placehold.co/600x400/fef2f2/dc2626?text=%E2%9C%95+Erreur" alt="Écran d'erreur"><figcaption><p>Et en cas d'erreur.</p></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

&#x20;

***

&#x20;

## <mark style="color:purple;">04</mark> · Cycle de vie d'une page

&#x20;

```mermaid
stateDiagram-v2
    direction LR
    [*] --> Brouillon
    Brouillon --> Relecture : Prête
    Relecture --> Brouillon : À corriger
    Relecture --> Publiée : Validée ✓
    Publiée --> Archivée
    Archivée --> [*]
```

&#x20;

| Statut          | Signification                          |
| --------------- | -------------------------------------- |
| ⚪ Brouillon    | En cours d'écriture                    |
| 🟡 Relecture    | <mark style="color:orange;">En attente de validation</mark> |
| 🟢 Publiée      | <mark style="color:green;">Visible par tous</mark>          |
| ⚫ Archivée     | Conservée, mais masquée                |

&#x20;

***

&#x20;

## <mark style="color:purple;">05</mark> · Les couleurs, et quand les utiliser

&#x20;

{% hint style="success" %}
**Bonne pratique** — ce qu'il faut faire.
{% endhint %}

{% hint style="warning" %}
**Attention** — un piège fréquent à éviter.
{% endhint %}

{% hint style="danger" %}
**Danger** — action irréversible, à lire deux fois.
{% endhint %}

&#x20;

Dans le texte, reste sobre :

&#x20;

* <mark style="color:green;">**Vert**</mark> → validé, recommandé
* <mark style="color:orange;">**Orange**</mark> → à surveiller
* <mark style="color:red;">**Rouge**</mark> → interdit, dangereux
* <mark style="color:blue;">**Bleu**</mark> → terme clé, lien important

&#x20;

{% hint style="info" %}
**Règle d'or** : une couleur doit toujours **signifier** quelque chose. Jamais de couleur pour décorer.
{% endhint %}

&#x20;

***

&#x20;

## <mark style="color:purple;">06</mark> · Feuille de route

&#x20;

```mermaid
timeline
    title Prochaines versions
    T4 2026 : 🚀 Lancement public
            : Documentation complète
    T1 2027 : 🔌 Intégrations
            : API v2
    T2 2027 : 📱 Application mobile
```

&#x20;

***

&#x20;

<details>

<summary>💡 Pour aller plus loin</summary>

&#x20;

Les blocs repliables servent aux détails que la plupart des lecteurs n'ont pas besoin de voir tout de suite : options avancées, cas limites, historique.

&#x20;

</details>

&#x20;

{% hint style="info" %}
**Une question ?** Ouvre une issue sur le dépôt ou écris-nous sur le canal `#docs`.
{% endhint %}