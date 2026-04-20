\# 📐 Documentation des mesures DAX — Dashboard RH



> Ce fichier documente toutes les mesures DAX utilisées dans le dashboard RH Power BI.

> Chaque mesure inclut le code, une explication, et le contexte d'utilisation.



\---



\## 📁 Organisation



Toutes les mesures sont regroupées dans une table dédiée `\_Mesures` pour faciliter la navigation et la maintenance.



\---



\## 👥 1. Mesures Effectifs



\---



\### Nb Employés Total



```dax

Nb Employés Total = COUNTROWS(Employés)

```



\*\*Explication\*\* : Compte toutes les lignes de la table `Employés`, quel que soit le statut.

\*\*Utilisé dans\*\* : Page 1 — Carte KPI, calcul du Taux de Turnover.



\---



\### Nb Employés Actifs



```dax

Nb Employés Actifs =

CALCULATE(

&#x20;   COUNTROWS(Employés),

&#x20;   Employés\[Statut] = "Actif"

)

```



\*\*Explication\*\* : Filtre la table `Employés` pour ne compter que les employés dont le statut est "Actif". `CALCULATE` modifie le contexte de filtre pour appliquer cette condition.

\*\*Utilisé dans\*\* : Page 1 — Carte KPI principale, calcul du Taux d'absentéisme.



\---



\### Taux de Turnover



```dax

Taux de Turnover =

DIVIDE(

&#x20;   CALCULATE(

&#x20;       COUNTROWS(Employés),

&#x20;       Employés\[Statut] = "Démissionné"

&#x20;   ),

&#x20;   \[Nb Employés Total]

)

```



\*\*Explication\*\* : Divise le nombre d'employés démissionnés par le total des employés. `DIVIDE` est utilisé à la place de `/` pour éviter l'erreur de division par zéro — il retourne 0 si le dénominateur est vide.

\*\*Utilisé dans\*\* : Page 1 — Carte KPI.

\*\*Format\*\* : Pourcentage, 1 décimale.



\---



\## 💰 2. Mesures Masse Salariale



\---



\### Masse Salariale Mensuelle



```dax

Masse Salariale Mensuelle = SUM(Employés\[Salaire\_Mensuel])

```



\*\*Explication\*\* : Additionne tous les salaires mensuels de la table `Employés`. Se filtre automatiquement selon les slicers actifs (département, statut, etc.).

\*\*Utilisé dans\*\* : Page 1 — Carte KPI, graphique masse salariale par département.

\*\*Format\*\* : `#,##0 "FCFA"` via Outils de mesure.



\---



\### Salaire Moyen



```dax

Salaire Moyen = AVERAGE(Employés\[Salaire\_Mensuel])

```



\*\*Explication\*\* : Calcule la moyenne des salaires mensuels. Sensible au contexte de filtre — change automatiquement selon le département ou le type de contrat sélectionné.

\*\*Utilisé dans\*\* : Page 1 — Carte KPI.

\*\*Format\*\* : `#,##0 "FCFA"`.



\---



\## 📅 3. Mesures Absences



\---



\### Total Jours Absence



```dax

Total Jours Absence = SUM(Absences\[Nb\_Jours])

```



\*\*Explication\*\* : Somme simple du nombre de jours d'absence. Point d'entrée pour les autres mesures d'absentéisme.

\*\*Utilisé dans\*\* : Page 2 — Carte KPI, graphiques barres empilées et courbe.



\---



\### Taux Absentéisme %



```dax

Taux Absentéisme % =

DIVIDE(

&#x20;   \[Total Jours Absence],

&#x20;   \[Nb Employés Actifs] \* 22

)

```



\*\*Explication\*\* : Divise le total des jours d'absence par le nombre théorique de jours ouvrables (22 jours par mois × nombre d'employés actifs). Un taux de 5% signifie que 5% du temps de travail est perdu en absences.

\*\*Utilisé dans\*\* : Page 2 — Carte KPI centrale.

\*\*Format\*\* : Pourcentage, 1 décimale.



\---



\### Nb Absences Non Approuvées



```dax

Nb Absences Non Approuvées =

CALCULATE(

&#x20;   COUNTROWS(Absences),

&#x20;   Absences\[Approuvé] = "Non"

)

```



\*\*Explication\*\* : Compte uniquement les absences dont le champ `Approuvé` vaut "Non". Indicateur d'alerte RH pour détecter les absences injustifiées.

\*\*Utilisé dans\*\* : Page 2 — Carte KPI d'alerte, colonne de la matrice.



\---



\## ⭐ 4. Mesures Performance



\---



\### Score Moyen Performance



```dax

Score Moyen Performance =

AVERAGE(Évaluations\_Performance\[Score\_Performance])

```



\*\*Explication\*\* : Moyenne des scores de performance (échelle 1 à 5). Se filtre selon le département, l'année ou le niveau sélectionné dans les slicers.

\*\*Utilisé dans\*\* : Page 3 — Carte KPI, barres groupées, courbe d'évolution, scatter.



\---



\### % Objectifs Atteints



```dax

% Objectifs Atteints =

AVERAGE(Évaluations\_Performance\[Objectifs\_Atteints\_%])

```



\*\*Explication\*\* : Moyenne du pourcentage d'objectifs atteints par les employés évalués. Complète le score de performance pour une vision plus nuancée.

\*\*Utilisé dans\*\* : Page 3 — Carte KPI, axe Y du scatter.

\*\*Format\*\* : Pourcentage, 1 décimale.



\---



\### Nb Excellents



```dax

Nb Excellents =

CALCULATE(

&#x20;   COUNTROWS(Évaluations\_Performance),

&#x20;   Évaluations\_Performance\[Niveau] = "Excellent"

)

```



\*\*Explication\*\* : Compte les évaluations avec le niveau "Excellent" (score = 5). Indicateur positif pour identifier les meilleurs profils et les départements performants.

\*\*Utilisé dans\*\* : Page 3 — Carte KPI.



\---



\### Evolution Score



```dax

Evolution Score =

VAR ScoreAnneeActuelle = \[Score Moyen Performance]

VAR ScoreAnneePrec =

&#x20;   CALCULATE(

&#x20;       \[Score Moyen Performance],

&#x20;       SAMEPERIODLASTYEAR(Calendrier\[Date])

&#x20;   )

RETURN

DIVIDE(ScoreAnneeActuelle - ScoreAnneePrec, ScoreAnneePrec)

```



\*\*Explication\*\* : Mesure avancée qui compare le score moyen de l'année sélectionnée avec celui de l'année précédente.

\- `VAR` : déclare des variables locales pour rendre la mesure lisible

\- `SAMEPERIODLASTYEAR` : fonction Time Intelligence qui décale le contexte de filtre d'un an en arrière

\- `DIVIDE` : évite la division par zéro si l'année précédente n'a pas de données



\*\*Prérequis\*\* : Nécessite la table `Calendrier` reliée à `Évaluations\_Performance\[Date Eval]`.

\*\*Utilisé dans\*\* : Page 3 — Carte KPI d'évolution.

\*\*Format\*\* : Pourcentage, 1 décimale.



\---



\## 🧠 Concepts DAX clés utilisés



| Concept | Fonction(s) | Rôle |

|---|---|---|

| Contexte de filtre | `CALCULATE` | Modifier les filtres actifs dans une mesure |

| Agrégation | `SUM`, `AVERAGE`, `COUNTROWS` | Calculer des totaux et moyennes |

| Division sécurisée | `DIVIDE` | Éviter les erreurs #DIV/0! |

| Time Intelligence | `SAMEPERIODLASTYEAR` | Comparer sur des périodes temporelles |

| Variables | `VAR ... RETURN` | Améliorer la lisibilité et les performances |

| Filtrage conditionnel | `FILTER` | Appliquer des conditions complexes |



\---



\## 💡 Bonnes pratiques appliquées



\- ✅ Toutes les mesures sont dans la table `\_Mesures` — jamais dans les tables de données

\- ✅ `DIVIDE` systématiquement à la place de `/` pour éviter les erreurs

\- ✅ Variables `VAR` utilisées pour les mesures complexes — plus lisibles et plus rapides

\- ✅ Format défini directement dans \*\*Outils de mesure\*\* — jamais avec `FORMAT()` en DAX

\- ✅ Noms de mesures en français avec espaces — plus lisibles dans les visuels



\---



\*Dashboard réalisé avec Power BI Desktop — Données RH fictives à des fins de démonstration.\*

