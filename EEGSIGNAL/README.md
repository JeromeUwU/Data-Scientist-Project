# EEG  — Signal, Features, Indices, Classification

## Mini-projets pour apprendre à traiter de l’EEG, en extraire des features interprétables et construire des indices utilisables en clinique ou en démo temps réel. Deux jeux de données sont utilisés :

- DEAP (extrait sans labels) — entraînement “signal-first” : filtres, fenêtres, band-powers (θ/α/β), PAF, θ/β frontal, heatmaps, stabilité.

- Muse Emotions (features + labels) — entraînement “supervisé” : standardisation, LDA/LogReg, permutation importance, indice LDA 1D (“Emotion Index”).

- But : se constituer un socle EEG/ML propre (pipeline + évaluation + indice prêt temps réel).

## 🔎 Méthode (commune)

- Sanity : visualiser des traces 5–10 s et une PSD (Welch) 30 s (occipital vs frontal) pour situer l’énergie (θ/α/β) et repérer des artefacts.

- Prétraitement : notch 50 Hz, passe-bande 1–40 Hz, re-référencement moyen.

- Fenêtrage : fenêtres de 2 s (overlap 50 %).

- Features “EEG-prêtes” :

  - Band-powers θ(4–7), α(8–13), β(13–30) par canal (Welch),

  - PAF (Peak Alpha Frequency) sur les canaux postérieurs,

  - Agrégats : α occipital, θ/β frontal,

- Permutation importance ; production d’un indice 1D (score LDA PC1).

## 📚 Résultats principaux

### A) DEAP (extrait sans labels) — “signal & features”

 - Pipeline opérationnel : filtres -> 2 s -> θ/α/β, PAF, α occipital, θ/β frontal.

 - Visualisations : stabilité des indices (séries temporelles), heatmaps “fenêtres × canaux”, PCA des features.

 - Utilité :  indices interprétables prêts à être utilisés en boucle (calcul rapide, <300 ms).

 - Limite : pas de labels dans le CSV -> pas d’évaluation supervisée. 


### B) Muse Emotions (features + labels) — “supervisé propre”

 - Baseline (5-fold CV + test 20 %) 

 - Accuracy = 0.79, F1_macro = 0.79 (LDA/LogReg).

 - NEUTRAL est mieux séparé ; POSITIVE/NEGATIVE se chevauchent (classique).

 - Variables importantes : surtout statistiques temporelles (familles mean_*, min_q_*, mean_d_*), les fft_* arrivent moins en tête.

 - Indice composite : Emotion Index = score LDA (PC1), utilisable comme jauge 1D (calcul très rapide).

## ⚠️ Limites & honnêteté scientifique

- DEAP (extrait) : pas de subject/trial/label -> pas de validation sujet-indépendante ici.

- Muse : dataset déjà featurisé, seulement 2 sujets et pas de champ subject -> possible sur-apprentissage au profil individuel ; les noms de variables ne se mappent pas clairement aux bandes/canaux -> on parle d’indices discriminants, pas de neuromarqueurs physiologiques.

- L’indice LDA est un composite statistique (interprétable ML), pas un biomarqueur neuro validé.


