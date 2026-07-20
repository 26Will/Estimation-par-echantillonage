# 📐 Estimation par échantillonnage — Intervalles de confiance & tests de conformité

SAE « Estimation par échantillonnage » — BUT Science des Données, IUT de Paris (2025–2026)

Étude du comportement des intervalles de confiance et des tests de conformité sur la moyenne, par simulation numérique sous R, puis application sur un jeu de données réel d'observations stellaires.

**Auteurs :** William Houblon-Tchagam, Florent Danlos, Nathan Sokol

---

## 🎯 Objectifs

- Illustrer par **simulation numérique** le comportement des intervalles de confiance (IC) selon différentes lois (Gaussienne, Bernoulli, Poisson, Exponentielle, Uniforme) et différentes tailles d'échantillon
- Implémenter et interpréter le **test de Student** (variance connue/inconnue, grands échantillons) par simulation
- Appliquer ces outils sur un cas réel : estimation et test de conformité sur la moyenne de la variable **Amag** (magnitude absolue des étoiles), à partir de deux échantillons de tailles 30 et 100

## 🧪 Partie 1 — Intervalles de confiance par simulation

Pour chaque loi étudiée, une fonction R renvoie la proportion d'IC contenant la vraie valeur du paramètre (sur `N` échantillons de taille `n`, au risque `alpha`), avec visualisation graphique des intervalles.

| Cas | Fonction R |
|---|---|
| Gaussien, variance connue | `ICNormaleVarCon` |
| Gaussien, variance inconnue | `ICNormaleVarInc` |
| Loi de Bernoulli (proportion) | `ICbernoulli` |
| Loi de Poisson | `ICpoisson` |
| Loi Exponentielle | `ICexponentielle` |
| Loi Uniforme [0, θ] | `ICuniforme` |
| Grands échantillons, variance connue | `ICVarConGrandsEch` |
| Grands échantillons, variance inconnue | `ICVarIncGrandsEch` |

## 🧪 Partie 2 — Tests de conformité sur la moyenne

- Simulation du **test de Student** (statistique `Tn`) dans les cas : variance connue, variance inconnue, grand échantillon non gaussien (Normale, Bernoulli, Poisson)
- Application sur deux jeux de données réels de mesures de balles de ping-pong (`Sample20.txt`, `Sample100.txt`, variable `diam`), en comparant la fonction `t.test` de R à une implémentation manuelle du test

## 📊 Partie 3 — Étude de cas : magnitude absolue des étoiles (Amag)

Population de référence : `Star39552balanced.csv` (39 552 étoiles). Variable étudiée : **Amag** (magnitude absolue) — H₀ : µ = 16, sur deux échantillons (`amag30_3.txt`, `amag100_3.txt`).

### Résultats

| | n = 30 | n = 100 |
|---|---|---|
| Moyenne observée | 15,7099 | 16,3279 |
| Écart-type | 2,7227 | 2,1201 |
| IC à 95 % | [14,7356 ; 16,6844] | [15,9123 ; 16,7434] |
| IC à 99 % | [14,4294 ; 16,9903] | [15,7818 ; 16,8740] |
| Statistique de test \|tₙ\| | 0,584 | 1,547 |
| p-valeur | 0,5595 | 0,1220 |
| Décision (α = 5 % et 1 %) | H₀ non rejetée | H₀ non rejetée |

### Démarche

- Statistiques descriptives (boxplot, histogramme, résumé) pour chaque échantillon
- Construction manuelle des IC à 95 % et 99 % (`IC(µ) = X̄ₙ ± cα × S/√n`)
- Test bilatéral H₀ : µ = 16 contre H₁ : µ ≠ 16, par comparaison au quantile, par la p-valeur, et via l'appartenance de µ₀ à l'IC
- Validation croisée avec la fonction `t.test` de R (loi de Student)
- Visualisation de la statistique de test observée face aux zones de rejet théoriques

### Conclusion

Les deux échantillons sont **statistiquement compatibles avec µ = 16**, sans rejet de H₀ à 5 % ni à 1 %. L'augmentation de la taille d'échantillon (30 → 100) illustre la théorie : les IC se resserrent (largeur divisée par ~1,8), la p-valeur diminue (0,56 → 0,12, signe d'une puissance de test accrue) et la distribution se rapproche davantage de la loi normale.

## 🛠️ Technologies

- **R** / RStudio
- Packages `matrixStats`, `dplyr`
- Fonctions maison (`FonctionsICtests.R`) chargées via `source()`

## 📁 Structure du dépôt

```
├── data/
│   ├── Star39552balanced.csv
│   ├── amag30_3.txt
│   └── amag100_3.txt
├── scripts/
│   └── analyse_amag.R          # Script du compte-rendu (IC, tests, graphiques)
├── compte-rendu/                # Compte-rendu Word/PDF
└── README.md
```

## 📄 Livrable

Compte-rendu (Word → PDF) commentant les sorties R et graphiques, pour les échantillons de taille 30 et 100 sur la variable Amag.

---

*Projet réalisé dans le cadre de la SAE « Estimation par échantillonnage » (Partie 2) — BUT Science des Données, IUT de Paris – Rives de Seine, année 2025–2026.*
