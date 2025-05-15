# HackatonIBM
## Présentation des jeux de données 📊

Pour ce data challenge, nous avons d’abord disposé de trois jeux de données au format CSV :  
- *cards_data.csv* , regroupant les informations relatives aux cartes (type, date d’émission, limites, etc.)  
- *users_data.csv* , contenant les attributs des utilisateurs (âge, genre, localisation, statut)  
- *transactions_train.csv*, listant les opérations effectuées (montant, date, moyen de paiement, marchand)  

Chacun de ces fichiers présentait des colonnes complémentaires ; nous avons donc procédé à leur fusion (merge) sur les clés communes (ID carte et ID utilisateur) afin de constituer un seul jeu de données unifié, prêt pour l’analyse et le prétraitement. 🚀

## Étiquetage de la fraude 🚩

Les étiquettes de fraude (fraud/no-fraud) étaient, quant à elles, fournies dans un fichier JSON à part, nommé *train_fraud_labels.json* 🗂️.  
Ce fichier associe à chaque identifiant de transaction son label de fraude, nous permettant ainsi de superviser l’apprentissage des modèles de détection. 🔍
# 📊 Résumé des Performances du Modèle LGBM – Détection de Fraude

**🔢 Nombre d'instances évaluées** : 21 000  
**🧠 Algorithme utilisé** : LightGBM (LGBMClassifier)  
**🧮 Nombre de caractéristiques** : 46  
**🎯 Colonne cible** : `target` (1 = fraude, 0 = non-fraude)

---

## Feature Engineering 🛠️

- Suppression des colonnes inutiles, telles que **cvv** 🔒 (risque de confidentialité) et autres attributs peu informatifs  
- Application du **One-Hot Encoding** pour les variables catégorielles, transformant chaque modalité en une colonne binaire distincte 🏷️

---

## ✅ Scores d'Évaluation

| **Métrique**               | **Holdout** | **Validation Croisée** |
|----------------------------|-------------|-------------------------|
| **Exactitude (Accuracy)**  | 0.999       | 0.999                   |
| **AUC-ROC**                | 0.996       | 0.985                   |
| **Précision**              | 0.767       | 0.687                   |
| **Rappel (Recall)**        | 0.742       | 0.743                   |
| **F1-Score**               | 0.754       | 0.711                   |
| **Précision Moyenne (AP)** | 0.786       | 0.749                   |
| **Log Loss**               | 0.003       | 0.004                   |

---

## 📌 Matrice de Confusion

| Observé       | Prédit 1 | Prédit 0 | % Correct |
|---------------|----------|----------|-----------|
| 1 (Fraude)    | 23       | 8        | 74.2%     |
| 0 (Non-fraude)| 7        | 20 962   | 100.0%    |

---

## 🧠 Interprétation des Résultats

- **Précision élevée** : Le modèle atteint une précision de 76.7% pour la classe fraude, ce qui indique une bonne capacité à identifier les transactions frauduleuses.
- **Rappel solide** : Avec un rappel de 74.2%, le modèle détecte une proportion significative des fraudes réelles.
- **AUC-ROC élevé** : Un score AUC-ROC de 0.996 en holdout suggère une excellente capacité de discrimination entre les classes.
- **Log Loss faible** : Une perte logarithmique de 0.003 indique que le modèle est bien calibré et confiant dans ses prédictions.

---

## 🛠️ Recommandations

- **Analyse des erreurs** : Étudiez les cas de fausses alertes (faux positifs) et de fraudes non détectées (faux négatifs) pour identifier des motifs ou des caractéristiques communes.
- **Ajustement du seuil de décision** : En fonction de la tolérance au risque, envisagez d'ajuster le seuil de classification pour équilibrer davantage la précision et le rappel.
- **Surveillance continue** : Mettez en place une surveillance continue des performances du modèle pour détecter toute dérive ou changement dans les données entrantes.

Cette premiere pipeline permet d'avoir les meilleurs compromis entre la précision et le rappel, cependant étant donné le problème il peut être pertinent de se concentrer sur le rappel.
Voici les performances d'une autre pipeline qui permet d'atteindre un rappel parfait mais une précision faible.
(peut-être mieux en function de l'usage du modèle)

# 📊 Model Summary – Decision Tree Classifier

## 🧪 Experiment Details

- **Prediction column**: `target`
- **Algorithm**: Decision Tree Classifier
- **Number of features**: 76
- **Evaluation instances**: 21,000


---

## 📈 Evaluation Metrics

### ✅ Holdout Metrics (single train/test split)

| Metric              | Value  |
|---------------------|--------|
| Accuracy            | 0.860  |
| Area under ROC      | 0.936  |
| Precision           | 0.010  |
| Recall              | 1.000  |
| F1 Score            | 0.021  |
| Average Precision   | 0.011  |
| Log Loss            | 0.307  |

---

### 🔁 Cross-Validation Metrics

| Metric              | Value  |
|---------------------|--------|
| Accuracy            | 0.865  |
| Area under ROC      | 0.914  |
| Precision           | 0.011  |
| Recall              | 0.954  |
| F1 Score            | 0.021  |
| Average Precision   | 0.011  |
| Log Loss            | 0.299  |

---

## 📉 Confusion Matrix Summary (Holdout)

| Actual \ Predicted | 1 (Fraud) | 0 (Non-fraud) | % Correct |
|--------------------|-----------|---------------|-----------|
| 1 (Fraud)          | 31        | 0             | 100.0%    |
| 0 (Non-fraud)      | 2937      | 18,032        | 86.0%     |

- **Class 1 (Fraud)** Recall: **100%**
- **Very low precision (1%)**: The model detects all fraud cases, but with many false positives.

---

## 📝 Interpretation

- The Decision Tree model **detects all fraud cases (100% recall)**, which is important in fraud detection.
- However, the **precision is extremely low (1%)**, meaning it misclassifies many normal transactions as fraud.
- This leads to a **very high number of false positives**, which may cause operational inefficiency or alert fatigue.

👉 Consider using more advanced models (like LGBM or Random Forest) or adding cost-sensitive learning to improve the precision.