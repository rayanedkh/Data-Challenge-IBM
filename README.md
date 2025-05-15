# HackatonIBM
## Présentation des jeux de données 📊

Pour ce data challenge, nous avons d’abord disposé de trois jeux de données au format CSV :  
- *cards_data.csv* 🃏, regroupant les informations relatives aux cartes (type, date d’émission, limites, etc.)  
- *users_data.csv* 👤, contenant les attributs des utilisateurs (âge, genre, localisation, statut)  
- *transactions_train.csv* 💸, listant les opérations effectuées (montant, date, moyen de paiement, marchand)  

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


