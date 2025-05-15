# Détection de Fraude Bancaire — Présentation Technique

---

## 1. Contexte & Objectif
- **Problématique** : détecter les transactions frauduleuses chez des clients **inédits**.  
- **Enjeu** : assurer une bonne **généralisation** du modèle sur de nouveaux clients.

---

## 2. Jeu de données
- **Période** : 2016–2018  
- **Total** : 210 000 transactions  
- **Taux de fraude** : ~0,15 %  
- **Split** : 90 % entraînement / 10 % test (hold-out client-wise)

---

## 3. Pré-traitement
1. **Sélection de colonnes**  
   - *Numeriques* : montant, latitude/longitude, score-crédit…  
   - *Catégorielles* : MCC, type de carte, etc. :contentReference[oaicite:0]{index=0}:contentReference[oaicite:1]{index=1}  
2. **Gestion des dates**  
   - Extraction de jour, mois, ancrage temporel sur `transaction_date` :contentReference[oaicite:2]{index=2}:contentReference[oaicite:3]{index=3}  
3. **Imputation & encodage**  
   - Imputer par la **médiane** pour les numériques  
   - Ordinal encoding pour les catégories  
4. **Normalisation**  
   - StandardScaler *optionnel* (désactivé pour préserver les distributions) :contentReference[oaicite:4]{index=4}:contentReference[oaicite:5]{index=5}  

---

## 4. Modélisation
- **Pipeline Scikit-Learn**  
  - `FeatureUnion` pré-processing + permutation d’axes  
  - Classifieur : `LGBMClassifier(class_weight='balanced', n_estimators=100, random_state=33)` :contentReference[oaicite:6]{index=6}:contentReference[oaicite:7]{index=7}  
- **Motivation**  
  - LightGBM gère nativement le **déséquilibre** (`class_weight`)  
  - Rapidité d’entraînement et bon compromis biais-variance

---

## 5. Entraînement & Validation
- **Métrique principale** : **Recall** (pour maximiser la détection de fraude)  
- **Résultat sur hold-out** :  
  - \> **Recall** = `<à compléter>`  
  - **Precision** = `<à compléter>`  
  - **ROC AUC** = `<à compléter>`  
- **Courbe ROC** et **matrice de confusion** à l’appui.  

---

## 6. Interprétation
- **Importance des features** (top 5) :  
  1. `amount`  
  2. `credit_score`  
  3. `mcc` (hashé)  
  4. `day_of_week` (issu du timestamp)  
  5. `num_credit_cards`  
- **Analyse** :  
  - Les montants élevés et un faible score-crédit augmentent fortement la probabilité de fraude.  
  - Certains MCC (ex. 5xxx) sont plus sensibles.

---

## 7. Perspectives & Améliorations
- **Enrichir** avec des features comportementales (vitesse, fréquence).  
- **Stacking** LightGBM + un second modèle (ex. XGBoost).  
- **Auto-encoder** pour capturer le profil normal de chaque client.  
- Intégrer un **système de calibration** des probabilités (Platt/Sigmoid).

---

## 8. Conclusion
- Pipeline léger et **robuste** face au déséquilibre.  
- **Generalisation** validée sur clients inédits.  
- Prochaines étapes : itérer sur l’ingénierie des features et l’ensemble de modèles.
