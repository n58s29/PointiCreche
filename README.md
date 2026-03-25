# 🏠 PointiCrèche v4 — Conformité pointage & impact PSU 2025

**Outil d'analyse des pointages en crèche (EAJE) pour piloter le taux de facturation, détecter les écarts contrat et estimer l'impact sur la PSU.**

Un seul fichier HTML, zéro dépendance, zéro installation. Fonctionne en local dans n'importe quel navigateur.

---

## 🎯 Le problème

En crèche, les parents doivent pointer (badger) à l'arrivée et au départ de leur enfant. En pratique :

- Certains **oublient de scanner** (matin, soir, ou les deux)
- Certains **scannent au mauvais moment** (après avoir déposé l'enfant le matin, ou avant de l'avoir récupéré le soir)
- Certains **arrivent en retard** ou **partent en avance** par rapport au contrat

Ces écarts ont un impact direct sur le **taux de facturation** (heures facturées ÷ heures réalisées), qui conditionne le montant de la **PSU** (Prestation de Service Unique) versée par la CAF. Depuis le 1er janvier 2025, le barème PSU est **linéarisé** : au-delà de 107%, le prix plafond décroît progressivement.

**Résultat :** quelques centaines d'heures d'écart peuvent coûter des dizaines de milliers d'euros de subventions à la structure.

---

## 📊 Ce que fait l'outil

### ⚙️ Config structure
Paramétrez votre crèche une seule fois :
- Nombre de places (arrêté PMI)
- Fourniture couches & repas (change le barème PSU)
- Charges de fonctionnement annuelles
- Participations familiales annuelles
- Taux de régime général
- Nombre d'enfants inscrits
- Journées pédagogiques (max 3/an)
- Tolérance de pointage (10 min par défaut, conforme FAQ PSU)

### 📥 Import
- **CSV pointeuse** : `Date;Heure;Enfant;Sens (A/D)` — l'export standard de la badgeuse
- **CSV contrats** : `Enfant;Jours;Heure_Arrivée;Heure_Départ;Taux_Horaire;Type` — les contrats individuels

### 📊 Écarts & PSU
Le tableau de bord principal :
- **Taux de facturation global** avec zone de couleur (≤107% = vert, >107% = orange)
- **Estimation PSU 2025** calculée avec le barème linéarisé (circulaire CNAF 2024-160)
- **Détail calcul** : prix de revient, prix plafond, PSU unitaire, financement journées pédagogiques, heures de préparation accueil
- **Manque à gagner** estimé par rapport à un taux de facturation idéal
- **Conformité par famille** : heures contrat vs heures pointées, avec jauge visuelle et tri par sévérité
- **Export CSV** du rapport complet

### 🔴 Absences de pointage
Liste des familles avec des oublis de scan récurrents :
- Journées sans aucun scan
- Arrivées non scannées
- Départs non scannés
- Taux d'oubli par famille

> **Règle appliquée** : en l'absence de pointage, les heures contrat sont automatiquement comptées (présence supposée). Mais la structure doit pouvoir justifier de pointages exacts à la minute en cas de contrôle CAF (conservation 6 ans).

### 👶 Détail famille
Vue journalière complète pour chaque enfant :
- Horaires de scan vs horaires contrat (code couleur)
- Delta en minutes par jour
- Filtres : anomalies, sous-contrat, oublis

---

## 🧮 Calcul PSU 2025 intégré

L'outil applique le barème PSU en vigueur depuis le 01/01/2025 (circulaire CNAF 2024-160) :

| Taux de facturation | Prix plafond (couches+repas) | Prix plafond (sans) |
|---|---|---|
| ≤ 107% | 10,05 €/h | 9,72 €/h |
| 107% à 120% | 21,96 − 11,13 × Tx fact. | 21,63 − 11,13 × Tx fact. |
| ≥ 120% | 8,60 €/h | 8,27 €/h |

**PSU = 66% × min(prix de revient, prix plafond) × heures facturées × taux RG**

Le financement des **journées pédagogiques** (max 3/an × 10h × nb places × PSU unitaire × taux RG) et des **heures de préparation à l'accueil** (nb inscrits × 6h × PSU unitaire × taux RG) est également calculé.

---

## 🚀 Utilisation

### Option 1 : Données démo
Ouvrez `pointicreche-v4.html` dans votre navigateur et cliquez sur **🎲 Démo** pour charger 49 familles simulées sur février 2026 avec des profils variés (oublis fréquents, retards, scans décalés, familles exemplaires).

### Option 2 : Vos données réelles
1. Exportez le CSV de votre pointeuse
2. Préparez le CSV contrats (ou utilisez les contrats par défaut)
3. Paramétrez votre structure dans l'onglet Config
4. Importez les deux fichiers

### Format CSV pointeuse attendu

```
Date;Heure;Enfant;Sens
02/02/2026;07:28;DUPONT Léa;A
02/02/2026;17:55;DUPONT Léa;D
```

- **Séparateur** : `;` (configurable)
- **Date** : JJ/MM/AAAA (configurable)
- **Heure** : HH:MM ou HHhMM
- **Sens** : A (arrivée) ou D (départ)

### Format CSV contrats attendu

```
Enfant;Jours;Heure_Arrivée;Heure_Départ;Taux_Horaire_EUR;Type_Contrat
DUPONT Léa;Lundi, Mardi, Mercredi, Jeudi, Vendredi;07:30;18:00;3.74;Temps plein
```

---

## 📁 Fichiers

| Fichier | Description |
|---|---|
| `pointicreche-v4.html` | L'outil complet (single-file, zéro dépendance) |
| `pointeuse_fevrier_2026.csv` | Données de pointeuse démo (49 familles, 20 jours ouvrés) |
| `contrats_fevrier_2026.csv` | Contrats démo (jours, horaires, taux variés) |

---

## 📋 Références réglementaires

- **Circulaire CNAF 2024-160** — Réforme PSU au 01/01/2025 (linéarisation du taux de facturation)
- **Circulaire CNAF 2024-123** — Journées pédagogiques et heures de préparation accueil
- **Lettre circulaire CNAF 2014-009** — Règles de contractualisation et facturation PSU
- **IT CNAF 2019-138** — Plancher et plafond des participations familiales
- **FAQ Règlement de fonctionnement EAJE** — Pointage, tolérance, arrondis, conservation des données

---

## ⚠️ Avertissements

- Cet outil est un **aide au pilotage** pour les directrices et directeurs de crèche. Il ne se substitue pas au logiciel de gestion agréé pour les déclarations CAF.
- Les estimations PSU sont indicatives et basées sur les paramètres saisis. Le calcul définitif est réalisé par la CAF sur les données réelles annuelles.
- Les données restent **100% en local** dans votre navigateur. Aucune donnée n'est envoyée à un serveur.

---

## 🛠️ Technique

- **HTML/CSS/JS** single-file — aucun framework, aucune dépendance
- Fonctionne offline, pas besoin de serveur
- Testé sur Chrome, Firefox, Safari, Edge
- Responsive (desktop + tablette)
- Fonts : [DM Sans](https://fonts.google.com/specimen/DM+Sans) + [Fraunces](https://fonts.google.com/specimen/Fraunces) (Google Fonts)

---

## 📝 Licence

Usage libre pour les structures d'accueil de jeunes enfants. Pas de garantie.

---

> *PointiCrèche — Quand les parents scannent mal, la CAF ne paie pas.*
