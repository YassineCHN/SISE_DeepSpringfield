# 🍩 DeepSpringfield — Détection de personnages des Simpsons

Projet de **détection d'objets** réalisé dans le cadre du cours de Deep Learning (Master 2 SISE — Université Lumière Lyon 2).

L'objectif est de détecter et localiser automatiquement les 5 personnages principaux des Simpsons (**Bart, Homer, Lisa, Maggie, Marge**) dans des images, en utilisant **YOLOv8** et le **transfer learning** depuis des poids pré-entraînés sur COCO.

---

## 📁 Structure du dépôt

```
SISE_DeepSpringfield/
├── README.md
├── dataset/                          # Jeu de données au format YOLOv8
│   ├── data.yaml                     # Configuration des classes
│   ├── README.roboflow.txt
│   ├── train/
│   │   ├── images/
│   │   └── labels/
│   ├── valid/
│   │   ├── images/
│   │   └── labels/
│   ├── test/
│   │   ├── images/
│   │   └── labels/
│   └── zip/
│       └── dataset_deepspringfield.zip
├── docs/
│   ├── DeepSpringfield.pdf           # Rapport final
│   └── media/                        # Figures générées par les notebooks
├── models/
│   ├── best_simpsons_model.pt        # Poids du meilleur modèle (run D)
│   └── experiments_results.csv       # Tableau comparatif des 4 runs
└── notebooks/
    ├── train_simpson.ipynb           # Notebook d'entraînement
    └── evaluation_simpson.ipynb      # Notebook d'évaluation
```

---

## ⚙️ Installation

Les notebooks sont conçus pour être exécutés sur **Google Colab** (GPU T4 recommandé).

La seule dépendance à installer est :

```bash
pip install ultralytics
```

Elle installe automatiquement `torch`, `torchvision` et toutes les dépendances nécessaires.

---

## 🚀 Ordre d'exécution

### 1. Notebook d'entraînement — `notebooks/train_simpson.ipynb`

> À exécuter en premier. Génère le meilleur modèle sauvegardé sur Google Drive.

1. Monter Google Drive et vérifier les chemins (adapter `DATASET_ROOT` si nécessaire)
2. Explorer le dataset (EDA)
3. Lancer les 4 expériences (~1h sur GPU T4)
4. Le meilleur modèle est automatiquement sauvegardé sous `models/best_simpsons_model.pt`

### 2. Notebook d'évaluation — `notebooks/evaluation_simpson.ipynb`

> À exécuter après l'entraînement. Charge `best_simpsons_model.pt` et évalue sur le jeu de test.

1. Monter Google Drive
2. Charger le modèle depuis `models/best_simpsons_model.pt`
3. Évaluer les métriques globales et par classe
4. Visualiser les prédictions, cas réussis et erreurs

---

## 🧪 Stratégie expérimentale

4 expériences comparant 2 architectures, 2 learning rates et 2 stratégies d'augmentation :

| Run | Modèle    | LR initial | Augmentation | mAP@50 val |
|-----|-----------|-----------|--------------|-----------|
| A   | YOLOv8n   | 1e-3      | Standard     | 0.757     |
| B   | YOLOv8n   | 5e-4      | Renforcée    | 0.734     |
| C   | YOLOv8s   | 1e-3      | Standard     | 0.832     |
| **D** | **YOLOv8s** | **5e-4** | **Renforcée** | **0.853** |

Le **run D** est sélectionné comme meilleur modèle.

---

## 📊 Résultats sur le jeu de test

| Métrique   | Valeur    |
|------------|-----------|
| mAP@50     | **0.877** |
| mAP@50-95  | 0.530     |
| Précision  | 0.843     |
| Rappel     | 0.828     |
| F1-score   | 0.836     |

**AP@50 par classe :**

| Classe | AP@50 |
|--------|-------|
| Bart   | 0.976 |
| Homer  | 0.981 |
| Lisa   | 0.844 |
| Maggie | 0.609 |
| Marge  | 0.973 |

18 images sur 25 sont parfaitement détectées (72%). Maggie est la classe la plus difficile en raison de sa petite taille dans le cadre.

---

## 📦 Dataset

- **163 images** collectées depuis diverses sources (série animée, illustrations, fan-arts, LEGO, Minecraft...)
- **416 annotations** au format YOLOv8 (coordonnées normalisées)
- Annoté avec [Roboflow](https://roboflow.com)
- Split : 114 train / 24 validation / 25 test

---

## 🔧 Hyperparamètres principaux

| Paramètre        | Valeur  |
|------------------|---------|
| Image size       | 640×640 |
| Batch size       | 16      |
| Epochs max       | 80      |
| Early stopping   | 15      |
| Optimizer        | AdamW   |
| Weight decay     | 5e-4    |
| Warmup epochs    | 3       |

---

**Auteurs :** Miléna GORDIEN-PIQUET & Yassine CHENIOUR — 2025-2026
