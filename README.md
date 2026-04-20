\# 📊 Dashboard RH — Power BI



> Tableau de bord interactif de gestion des ressources humaines, couvrant les effectifs, les absences et les performances des employés.



!\[Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge\&logo=powerbi\&logoColor=black)

!\[DAX](https://img.shields.io/badge/DAX-185FA5?style=for-the-badge\&logoColor=white)

!\[Power Query](https://img.shields.io/badge/Power%20Query-217346?style=for-the-badge\&logoColor=white)

!\[Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge\&logo=microsoftexcel\&logoColor=white)



\---



\## 🎯 Objectif du projet



Ce dashboard RH a été conçu pour permettre aux équipes de direction et aux responsables RH de :



\- Suivre les effectifs en temps réel par département, statut et type de contrat

\- Analyser l'absentéisme et identifier les tendances par période et par équipe

\- Évaluer les performances des employés sur 3 ans (2022–2024)

\- Prendre des décisions basées sur les données sans manipulation de fichiers Excel



\---



\## 🖼️ Aperçu du dashboard



| Page 1 — Vue générale | Page 2 — Absences | Page 3 — Performance |

|---|---|---|

| !\[Page 1](screenshots/page1\_vue\_generale.png) | !\[Page 2](screenshots/page2\_absences.png) | !\[Page 3](screenshots/page3\_performance.png) |



> 💡 \*Remplace les images ci-dessus par tes captures d'écran réelles dans le dossier `/screenshots`\*



\---



\## 📁 Structure du projet



```

portfolio-powerbi-rh/

│

├── 📄 README.md

├── 📊 RH\_Dashboard.pbix

├── 📂 data/

│   └── RH\_Dataset\_PowerBI.xlsx

├── 📂 screenshots/

│   ├── page1\_vue\_generale.png

│   ├── page2\_absences.png

│   └── page3\_performance.png

└── 📄 DAX\_mesures.md

```



\---



\## 🗄️ Source de données



Le jeu de données est un fichier Excel structuré en \*\*4 tables\*\* :



| Table | Description | Lignes |

|---|---|---|

| `Employés` | Données personnelles, poste, salaire, statut | 100 |

| `Absences` | Types d'absence, dates, durée, approbation | 200 |

| `Évaluations\_Performance` | Scores annuels 2022–2024 par employé | 300 |

| `Départements` | Budget, effectif cible, responsable | 8 |



\---



\## 🔗 Modèle de données



Le modèle suit une architecture en \*\*étoile\*\* :



```

Calendrier

&#x20;   │

&#x20;   ▼

Absences ◄──── Employés ────► Évaluations\_Performance

&#x20;                  │

&#x20;                  ▼

&#x20;            Départements

```



\- Toutes les relations sont \*\*actives\*\* avec une cardinalité `\*:1`

\- La table `Calendrier` est générée via `CALENDARAUTO()`

\- Le filtrage croisé se propage de `Employés` vers les tables de faits



\---



\## ⚙️ Compétences démontrées



\### Power Query (ETL)

\- Importation et transformation de données multi-tables

\- Vérification et correction des types de colonnes

\- Création de colonnes personnalisées (Tranche d'âge, Date Eval)



\### Modélisation

\- Schéma en étoile avec table de dates dédiée

\- Gestion des relations actives/inactives

\- Colonnes calculées pour enrichir les dimensions



\### DAX

\- Mesures de base : `SUM`, `AVERAGE`, `COUNTROWS`

\- Mesures avec contexte de filtre : `CALCULATE`, `FILTER`

\- Time Intelligence : `SAMEPERIODLASTYEAR`

\- Gestion des erreurs : `DIVIDE` avec alternative

\- Variables DAX pour la lisibilité des mesures complexes



\### Visualisation

\- Cartes KPI, barres simples et groupées, courbes

\- Graphiques en anneau, nuage de points (scatter)

\- Matrice avec sous-totaux dynamiques

\- Slicers avec style Tuiles et filtre de période (Entre)

\- Interactions entre visuels configurées manuellement



\---



\## 📐 Mesures DAX principales



```dax

\-- Effectifs

Nb Employés Actifs =

CALCULATE(COUNTROWS(Employés), Employés\[Statut] = "Actif")



\-- Masse salariale

Masse Salariale Mensuelle = SUM(Employés\[Salaire\_Mensuel])



\-- Turnover

Taux de Turnover =

DIVIDE(

&#x20;   CALCULATE(COUNTROWS(Employés), Employés\[Statut] = "Démissionné"),

&#x20;   \[Nb Employés Total]

)



\-- Absentéisme

Taux Absentéisme % =

DIVIDE(SUM(Absences\[Nb\_Jours]), \[Nb Employés Actifs] \* 22)



\-- Performance

Evolution Score =

VAR ScoreActuel = \[Score Moyen Performance]

VAR ScorePrec = CALCULATE(

&#x20;   \[Score Moyen Performance],

&#x20;   SAMEPERIODLASTYEAR(Calendrier\[Date])

)

RETURN DIVIDE(ScoreActuel - ScorePrec, ScorePrec)

```



> 📄 Retrouve la documentation complète dans \[`DAX\_mesures.md`](DAX\_mesures.md)



\---



\## 🚀 Comment utiliser ce projet



1\. \*\*Clone\*\* le dépôt :

&#x20;  ```bash

&#x20;  git clone https://github.com/ton-username/portfolio-powerbi-rh.git

&#x20;  ```



2\. \*\*Ouvre\*\* `RH\_Dashboard.pbix` dans Power BI Desktop



3\. \*\*Relie\*\* la source de données vers le fichier `data/RH\_Dataset\_PowerBI.xlsx` si nécessaire :

&#x20;  \*Transformer les données → Paramètres de la source de données → Modifier\*



4\. \*\*Actualise\*\* le rapport — les visuels se chargent automatiquement



\---



\## 📬 Contact



Disponible pour des missions freelance en \*\*Business Intelligence\*\* et \*\*Data Analytics\*\*.



\- 💼 LinkedIn : \[ton-profil-linkedin]

\- 📧 Email : \[ton-email]

\- 🌍 Localisation : N'Djamena, Tchad



\---



\*Projet réalisé dans le cadre d'un parcours de perfectionnement Power BI — orienté usage freelance.\*

