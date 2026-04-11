---
doc_id: OPENCLAW_CHANTIER_LIFECYCLE
doc_type: workflow_rule
repo: openclaw
project: openclaw
module:
go_id: GO_OPENCLAW_UNIFORM_CONTINUITY_GOVERNANCE_01
status: reference
lifecycle_stage: governance
topic_keys:
  - openclaw
  - governance
  - chantier
  - lifecycle
surface: governance
source_kind: canonical
updated_at: 2026-04-11
links:
  - docs/governance/UNIFORM_CONTINUITY_METHOD.md
---

# CHANTIER_LIFECYCLE — openclaw

## Objet

Ce document fixe le cycle de vie canonique d’un chantier structuré.

---

## 1. Étapes du lifecycle

### 1.1 Intention
- idée de départ
- problème visé
- usage attendu
- valeur recherchée

### 1.2 Cadrage
- état de départ retenu
- objectif du lot
- non-objectifs
- critères PASS / FAIL

### 1.3 Plan
- étapes prévues
- zones de travail
- validations prévues
- risques

### 1.4 Exécution
- actions réellement faites
- preuves
- résultats
- incidents techniques

### 1.5 Validation / Décisions
- arbitrages structurants
- option retenue
- raison du choix
- impact

### 1.6 Closeout
- réalisé
- fichiers touchés
- validations exécutées
- limites restantes
- verdict PASS / FAIL

### 1.7 Reprise / Next
- point de reprise
- prochaine action
- suites naturelles
- candidats GO suivants si pertinents

---

## 2. Structure documentaire attendue

Pour un chantier structuré :
- `00_cadrage.md`
- `01_plan.md`
- `02_journal_technique.md`
- `03_decisions.md`
- `90_closeout.md`

---

## 3. Règles

### 3.1 Un chantier structuré = un dossier dédié

### 3.2 Le closeout n’est pas le journal

### 3.3 Les décisions ne doivent pas être noyées dans le factuel

### 3.4 La reprise doit rester lisible et actionnable

### 3.5 Un closeout doit alimenter la continuité quand pertinent

---

## 4. Statut

Statut :
- document transverse de référence
