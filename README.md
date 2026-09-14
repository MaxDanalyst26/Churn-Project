# 📊 Churn Prediction Project — Analyse de la rétention clients

## 🎯 Objectif du projet
Ce projet vise à prédire le **churn** (désabonnement) des clients d’un service de streaming à partir de données comportementales et transactionnelles.  
L’objectif est double :

1. **Identifier les clients à risque** grâce à un modèle de machine learning.  
2. **Quantifier le revenu mensuel menacé** pour prioriser les actions de rétention.

Ce projet illustre une approche complète : exploration, préparation des données, modélisation, évaluation, interprétation et analyse métier.

---

## 📁 Structure du dépôt

---

## 🧠 Description du dataset
Les données utilisées proviennent de Kaggle. Pour des raisons de licence elles n'ont pas été sauvegardées dans le Repository.
Elles ont été partagées en trois fichiers "test.csv", "train.csv", "data_descriptions.csv". Mais ce travail je me suis servis
que des fichiers train et description. 

📊 Données disponibles sur Kaggle :  
🔗 (https://www.kaggle.com/datasets/safrin03/predictive-analytics-for-customer-churn-dataset?utm_source=karina-datascientist.beehiiv.com&utm_medium=newsletter&utm_campaign=let-s-build-a-churn-prediction-model-together)

<h3>📥 Données</h3>
<p>
  <a href="https://www.kaggle.com/datasets/safrin03/predictive-analytics-for-customer-churn-dataset?utm_source=karina-datascientist.beehiiv.com&utm_medium=newsletter&utm_campaign=let-s-build-a-churn-prediction-model-together" target="_blank">
    👉 Cliquez ici pour accéder au dataset sur Kaggle
  </a>
</p>
Le dataset contient **243 787 clients** et **21 variables**, incluant :
Données

| Column Name                 | Type        | Data Type | Description |
|-----------------------------|-------------|-----------|-------------|
| AccountAge                  | Feature     | integer   | The age of the user's account in months. |
| MonthlyCharges              | Feature     | float     | The amount charged to the user on a monthly basis. |
| TotalCharges                | Feature     | float     | The total charges incurred by the user over the account's lifetime. |
| SubscriptionType            | Feature     | object    | The type of subscription chosen by the user (Basic, Standard, Premium). |
| PaymentMethod               | Feature     | string    | The method of payment used by the user. |
| PaperlessBilling            | Feature     | string    | Indicates whether the user has opted for paperless billing (Yes or No). |
| ContentType                 | Feature     | string    | The type of content preferred by the user (Movies, TV Shows, Both). |
| MultiDeviceAccess           | Feature     | string    | Indicates whether the user has access to the service on multiple devices (Yes or No). |
| DeviceRegistered            | Feature     | string    | The type of device registered by the user (TV, Mobile, Tablet, Computer). |
| ViewingHoursPerWeek         | Feature     | float     | The number of hours the user spends watching content per week. |
| AverageViewingDuration      | Feature     | float     | The average duration of each viewing session in minutes. |
| ContentDownloadsPerMonth    | Feature     | integer   | The number of content downloads by the user per month. |
| GenrePreference             | Feature     | string    | The preferred genre of content chosen by the user. |
| UserRating                  | Feature     | float     | The user's rating for the service on a scale of 1 to 5. |
| SupportTicketsPerMonth      | Feature     | integer   | The number of support tickets raised by the user per month. |
| Gender                      | Feature     | string    | The gender of the user (Male or Female). |
| WatchlistSize               | Feature     | float     | The number of items in the user's watchlist. |
| ParentalControl             | Feature     | string    | Indicates whether parental control is enabled (Yes or No). |
| SubtitlesEnabled            | Feature     | string    | Indicates whether subtitles are enabled (Yes or No). |
| CustomerID                  | Identifier  | string    | A unique identifier for each customer. |
| Churn                       | Target      | integer   | Indicates whether a user has churned (1) or not (0). |

Le churn est **déséquilibré** :  
- 81.9 % restent  
- 18.1 % quittent

---

## 🔍 Exploration des données (EDA)
Avant de débuter le projet, j'ai pris le temps d'explorer les données et de faire ressortir les premiers insights clés qui pourraient nous permettre de déceler la clientèle la plus à risque.

### Insights clés :
- Les clients **Basic** ont le taux de churn le plus élevé (~19.6 %).  

![Churn Rate par type de souscription](images/churn_rate_souscription.png)

- Les churners regardent **moins de contenu** (17.4 h/semaine vs 21.2 h).  

![Heures de visualisation par churners](images/visualisation_churn.png)

- Les comptes plus récents churnent davantage (45.7 mois vs 63.3 mois).

---

## 🛠️ Préparation des données
Par la suite j'ai effectué une préparation des données m'assurant au préalable que les données étaient propres, sans doublons et sans valeurs manquantes.

### Étapes principales :
- Suppression de `CustomerID`
- Encodage des variables catégorielles (OneHotEncoder)
- Standardisation des variables numériques (StandardScaler)
- Reconstruction du dataset final :  
  **colonnes numériques scalées + colonnes OneHot**
- Séparation de l'echantillon train/test : 80 % / 20 %

---

## 🤖 Modèles entraînés
Avant l'entrainement du modèle, 2 modèles me semblaient les plus adaptés pour le problème, compte tenu de la quantité de données mais aussi du fait qu'on devait classer deux catégories très déséquilibrées. Les deux modèles linéaires adaptés aux grands volumes sont :

### **1. Logistic Regression**
- `class_weight='balanced'`  
- `max_iter=3000`  
- Probabilités disponibles → idéal pour le churn  
- Très bon compromis précision / recall  

### **2. SGDClassifier (régression logistique)**
- `loss="log_loss"`  
- `early_stopping=True`  
- Très rapide sur grands datasets  
- Performances légèrement inférieures

---

## 📈 Résultats des modèles

### **Logistic Regression**
- Accuracy : **0.68**  
- Recall churn : **0.69**  
- AUC : **0.73**

### **SGDClassifier**
- Accuracy : **0.65**  
- Recall churn : **0.72**  
- AUC : **0.71**

👉 **Modèle retenu : Logistic Regression**  
Mon choix s'est porté sur le modèle de régression logistique car il offre le meilleur compromis entre recall, stabilité et interprétabilité.

---

## 📉 Matrice de confusion
On constate que le modèle de régression logistique permet de saisir correctement:
- **68 %** des clients qui restent  
- **69 %** des clients qui churnent  

Ce recall élevé sur la classe minoritaire est essentiel pour un cas de churn en entreprise.

---

## 💰 Analyse métier : Revenu à Risque
Sur la base des prédictions, j'ai pu obtenir les probabilités pour chaque client de se désabonner. Cela m'a permis d'estimer
quel serait le revenu potentiel (RevenueAtRisk) que la compagnie pourrait perdre si des clients résiliaient leur abonnement.
Pour chaque client du test :

RevenueAtRisk = Probabilité(churn = 1) x MonthlyCharges


### Résultats :
Nous avons pu obtenir le montant total mensuel espéré que la compagnie pourrait perdre si aucune campagne de rétention était
faite. Par la suite j'ai classé ces revenus par ordre décroissant pour pouvoir faire ressortir les 20 clients les plus susceptibles de se désabonner.
- **Revenu mensuel total à risque : 279 691 $**
- Liste des **20 clients prioritaires** générée automatiquement

Cette section montre comment le modèle peut être utilisé pour **prioriser les actions de rétention**.

---

## 🔍 Features les plus importantes

Top variables influençant le churn :

- `AccountAge`  
- `ViewingHoursPerWeek`  
- `MonthlyCharges`  
- `SupportTicketsPerMonth`  
- `ContentDownloadsPerMonth`  

![Features les plus influents](images/top_features.png)

Graphique inclus dans le notebook.

---

## 🚀 Améliorations possibles

- Tester des modèles non linéaires (XGBoost, LightGBM, CatBoost)
- Feature engineering (ratios, interactions)
- Calibration des probabilités
- Optimisation du seuil de classification selon le coût métier
- Déploiement API (FastAPI)

---

## 🏁 Conclusion

Ce projet démontre une approche complète de prédiction du churn :

- Analyse exploratoire  
- Préparation des données  
- Modélisation  
- Évaluation  
- Interprétation  
- Analyse métier (revenue at risk)



---
