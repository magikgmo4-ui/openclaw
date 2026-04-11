---
doc_id: OPENCLAW_STATUS_MODEL
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
  - status
  - workflow
surface: governance
source_kind: canonical
updated_at: 2026-04-11
links:
  - docs/governance/UNIFORM_CONTINUITY_METHOD.md
  - docs/governance/CHANTIER_LIFECYCLE.md
---

# STATUS_MODEL — openclaw

## Objet

Ce document fixe les statuts canoniques utilisables dans la méthode uniforme.

---

## 1. Statuts principaux

### `draft`
Travail non encore stabilisé.

### `candidate`
Suite naturelle ou chantier potentiel qualifié mais non ouvert.

### `active`
Chantier réellement en cours.

### `blocked`
Chantier ouvert mais actuellement bloqué.

### `pass`
Chantier ou lot clos avec résultat jugé valide pour le périmètre retenu.

### `fail`
Chantier ou lot clos avec résultat insuffisant ou invalide.

### `merged`
État utile quand le point dominant est l’intégration Git déjà mergée.

### `archived`
État clos sans reprise immédiate attendue.

### `reference`
Document de gouvernance, index ou artefact de référence stable.

### `deferred`
Sujet explicitement repoussé.

---

## 2. Règles d’usage

### 2.1 Un seul statut principal par artefact

### 2.2 `pass` et `fail` sont des états de fermeture

### 2.3 `reference` n’est pas un état de chantier actif

### 2.4 `candidate` n’est pas `active`

### 2.5 `blocked` conserve un point de reprise

---

## 3. Correspondances utiles

- gouvernance stable -> `reference`
- chantier ouvert -> `active`
- chantier interrompu -> `blocked`
- closeout validé -> `pass`
- closeout non validé -> `fail`
- suite qualifiée non ouverte -> `candidate`

---

## 4. Statut

Statut :
- document transverse de référence
