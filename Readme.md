# 📊 Analyse des Incidents Réseau & Activité des Attachements — Power BI Dashboard

## Description

Ce projet Power BI est construit sur une **source de données unique** (une seule table) et offre une vue analytique complète de l'activité opérationnelle sur le réseau ferroviaire francilien. Il couvre le suivi des incidents par station, la localisation des colis suspects, les résultats d'olfaction, la performance individuelle des agents, ainsi que l'évolution temporelle de l'activité par attachement.

---

## 📁 Structure du projet

```
├── rapport_incidents_reseau.pbix  # Fichier Power BI principal
└── README.md
```

> ⚠️ Ce projet repose sur **une seule table source** — aucune relation entre tables n'est requise.

---

## 📄 Pages du rapport

### 1. 📋 `Rapport d'Activité — Suivi des Attachements et Localisations`
Vue d'ensemble de la répartition du volume de travail par site et par agent.
- **Nombre de Ligne par Attachement** (histogramme) : Denfert Rochereau et Châtelet-les Halles concentrent le plus grand volume de lignes traitées
- **Nombre de Num fiche par Attachement** (courbe) : décroissance progressive du volume de fiches, Denfert Rochereau en tête
- **Nombre de Localisation colis par Année** (camembert) : répartition 2021–2026, avec 2024 dominant à 33%
- **Nombre de Commentaire par Nom et Prénom** (histogramme) : identification des agents les plus actifs (Marty Mac Fly et Porky Pig en tête)
- Analyse textuelle intégrée résumant les points clés

### 2. 📍 `Analyse des Localisations et Résultats d'Olfaction`
Détail de l'emplacement des interventions et de l'efficacité des levées de doute.
- **Nombre de Localisation colis par Localisation** (anneau) :
  - Rame/Train à quai : **48,22%**
  - Sur le quai : **33,73%**
  - Plateforme d'échange, Accès, Couloir : volume résiduel
- **Nombre d'Olfaction par Résultat olfaction** (graphique en aires) :
  - Majorité des interventions = **Non marquage** (indicateur de sécurité positif)
  - Cas de Marquage et Douteux extrêmement rares
- Analyse textuelle intégrée sur la répartition géographique et les résultats

### 3. 🚨 `Analyse des Incidents et État du Réseau`
Tableau de bord de surveillance de la fluidité du réseau et de la réactivité des équipes.
- **Nombre d'incident par Station** (histogramme) : Denfert-Rochereau et Bonne Nouvelle en tête (~20 et ~13 incidents)
- **Nombre de Localisation colis par Colonne / État Trafic** (camembert) :
  - **93,75% Perturbé** vs 6,25% Normal — forte période de perturbation sur la période analysée
- **Nombre d'incident par Année** (courbe) : fluctuation constante 2022–2024, tendance à la hausse des pics en 2025
- **Nombre d'intervention par Attachement** (histogramme) : filtré dynamiquement par le slicer Attachement
- **Slicer Attachement** (multi-sélection) : filtre interactif permettant de sélectionner une ou plusieurs gares simultanément
- Analyse textuelle intégrée décrivant les top stations et l'état du réseau

---

## 🗃️ Modèle de données

### Table unique — structure des colonnes utilisées

| Colonne | Type | Utilisation |
|---|---|---|
| `Attachement` | Texte | Gare de rattachement (filtre principal) |
| `Station` | Texte | Station d'incident |
| `Localisation colis` | Texte | Lieu de détection (Rame, Quai, Couloir…) |
| `Résultat olfaction` | Texte | Issue du contrôle (Non marquage, Marquage, Douteux) |
| `Colonne` | Texte | État du trafic (Perturbé / Normal) |
| `Date` / `Année` | Date | Axe temporel des analyses |
| `Nom` / `Prénom` | Texte | Identification de l'agent |
| `Num fiche` | Texte/Num | Identifiant de fiche d'intervention |
| `Ligne` | Texte | Ligne de transport concernée |
| `Commentaire` | Texte | Observations saisies par l'agent |

### Mesures DAX principales

```dax
-- Comptage des incidents
Nb_Incidents = COUNTROWS('table')

-- Comptage des interventions
Nb_Interventions = COUNTROWS('table')

-- Comptage des olfactions
Nb_Olfactions = COUNTROWS('table')

-- Comptage des commentaires
Nb_Commentaires = COUNTA('table'[Commentaire])

-- Comptage des num fiches
Nb_Fiches = DISTINCTCOUNT('table'[Num fiche])
```

---

## 🔽 Slicer — Filtre par Attachement (multi-sélection)

Le rapport intègre un **slicer multi-sélection** sur le champ `Attachement` permettant de filtrer simultanément plusieurs gares.

**Configuration du slicer :**
> Insérer → Slicer → glisser `[Attachement]` dans le champ
> Format → Paramètres du slicer → Style : **Liste** ou **Dropdown**
> Format → Sélection → activer **"Sélection multiple avec CTRL"** ou cocher **"Afficher le bouton Sélectionner tout"**

**Gares disponibles dans le filtre :**
Châtelet-les Halles, Denfert Rochereau, La Défense, Nation, Gare de l'Est, Massy Palaiseau, Noisy le Grand Mont d'Est, République, Stade de France, SNCF Gare de Lyon, SNCF Gare du Nord, SNCF Montparnasse, SNCF Chessy Marne-la-Vallée, SNCF Saint Lazare, Trocadéro, Balard, Saint-Denis Université, Nation_DB

---

## 🛠️ Prérequis

- **Power BI Desktop** (version récente recommandée)
- Aucun visual tiers requis — tous les visuels utilisés sont natifs Power BI :
  - Histogramme groupé
  - Graphique en courbes
  - Graphique en aires
  - Graphique en anneau / camembert
  - Slicer

---

## 🚀 Installation

```bash
git clone <url-du-repo>
cd <nom-du-repo>
```

1. Ouvrir `rapport_incidents_reseau.pbix` dans Power BI Desktop
2. Mettre à jour la source de données si nécessaire :
   > Transformer les données → Paramètres de la source de données
3. Cliquer sur **Actualiser** pour charger les données

---

## 📌 Remarques techniques

- **Source unique** : tout le rapport est construit sur une seule table — pas de modèle relationnel
- Le slicer `Attachement` propage le filtre sur **toutes les pages** du rapport si configuré en filtre de niveau rapport
- Les analyses textuelles intégrées dans chaque page sont des zones de texte statiques rédigées à partir des observations des données
- L'état "Perturbé/Normal" provient de la colonne `Colonne` — renommer cette colonne en `Etat_Trafic` est recommandé pour plus de clarté

---

## 👥 Contexte métier

Ce rapport permet aux responsables opérationnels de suivre en temps réel la charge de travail par gare, d'identifier les stations à forte concentration d'incidents, d'analyser la qualité des levées de doute (olfaction), et de mesurer l'implication individuelle des agents sur le réseau RATP/SNCF en Île-de-France.