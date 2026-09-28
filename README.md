# HEIA-IT — Cours

Bienvenue dans mes notes de cours de la HEIA-FR (filière Informatique).
Ce GitBook regroupe la théorie de chaque branche, organisée par matière puis par chapitre.

## Matières

| Matière | Description |
| --- | --- |
| [Téléinformatique](teleinformatique/README.md) | Réseaux, protocoles et communication de données |
| [Programmation](programmation/README.md) | Langages, algorithmes et bonnes pratiques |
| [Technique Numérique](technique-numerique/README.md) | Logique, systèmes numériques et électronique digitale |
| [Méthodologie et Sécurité IT](methodologie-securite-it/README.md) | Méthodes de travail, gestion de projet et sécurité informatique |

## Organisation du dépôt

```
.
├── README.md              # Page d'accueil (cette page)
├── SUMMARY.md             # Table des matières GitBook
├── .gitbook.yaml          # Configuration de la synchronisation GitBook
├── .gitbook/assets/       # Images et fichiers joints
├── teleinformatique/
│   └── README.md          # Introduction de la matière
├── programmation/
├── technique-numerique/
└── methodologie-securite-it/
```

## Conventions

- Chaque matière possède un `README.md` qui sert de page d'introduction.
- Un chapitre = un fichier `NN-nom-du-chapitre.md` (ex. `01-modele-osi.md`), numéroté pour garder l'ordre.
- Les noms de fichiers et de dossiers sont en minuscules, sans accents ni espaces (tirets à la place).
- Les images vont dans `.gitbook/assets/` et sont référencées en chemin relatif.
- Toute nouvelle page doit être ajoutée dans [`SUMMARY.md`](SUMMARY.md) pour apparaître dans GitBook.
