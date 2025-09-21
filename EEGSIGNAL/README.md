# EEG  — Signal, Features, Indices, Classification

## Mini-projets pour apprendre à traiter l’EEG, extraire des features interprétables et construire des indices utilisables. 

### Deux jeux de données sont utilisés :

- DEAP (extrait sans labels) — entraînement signal : filtres, fenêtres, band-powers (θ/α/β), PAF, θ/β frontal, heatmaps, stabilité.

- Muse Emotions (features + labels) — entraînement “supervisé” : standardisation, LDA/LogReg, permutation importance, indice LDA 1D (“Emotion Index”).

- But : se constituer un socle EEG/ML propre (pipeline + évaluation + indice prêt temps réel).

## 🔎 Méthode (commune)

- Sanity : visualiser des traces 5–10 s et une PSD (Welch) 30 s (occipital vs frontal) pour situer l’énergie (θ/α/β) et repérer des artefacts.

- Prétraitement : notch 50 Hz, passe-bande 1–40 Hz, re-référencement moyen.

- Fenêtrage : fenêtres de 2 s (overlap 50 %).

- Features EEG:

  - Band-powers θ(4–7), α(8–13), β(13–30) par canal (Welch),

  - PAF (Peak Alpha Frequency) sur les canaux postérieurs,

  - Agrégats : α occipital, θ/β frontal,

  - Permutation importance ; production d’un indice 1D (score LDA PC1).

## 📚 Résultats principaux

### A) DEAP (extrait sans labels) — “signal & features”

 - Pipeline opérationnel : filtres -> 2 s -> θ/α/β, PAF, α occipital, θ/β frontal.

 - Visualisations : stabilité des indices (séries temporelles), heatmaps “fenêtres × canaux”, PCA des features.

 - Utilité :  indices interprétables prêts à être utilisés en boucle.

 - Limite : pas de labels dans le CSV -> pas d’évaluation supervisée. 


### B) Muse Emotions (features + labels) — “supervisé propre”

 - Baseline (5-fold CV + test 20 %) 

 - Accuracy = 0.79, F1_macro = 0.79 (LDA/LogReg).

 - NEUTRAL est mieux séparé , POSITIVE/NEGATIVE se chevauchent.

 - Variables importantes : surtout statistiques temporelles.

 - Indice composite : Emotion Index = score LDA (PC1), utilisable comme jauge 1D (calcul très rapide).

## ⚠️ Limites 

- DEAP (extrait) : pas de subject/trial/label -> pas de validation sujet-indépendante ici.

- Muse : dataset déjà featurisé, seulement 2 sujets  -> possible sur-apprentissage au profil individuel , les noms de variables ne se mappent pas clairement aux bandes/canaux -> indices discriminants, pas de neuromarqueurs physiologiques explicit.

- L’indice LDA est un composite statistique (interprétable), pas un biomarqueur neuro validé.


