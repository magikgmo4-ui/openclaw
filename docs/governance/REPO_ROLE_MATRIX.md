---
doc_id: OPENCLAW_REPO_ROLE_MATRIX
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
  - repo_role
  - matrix
surface: governance
source_kind: canonical
updated_at: 2026-04-11
links:
  - docs/governance/UNIFORM_CONTINUITY_METHOD.md
---

# REPO_ROLE_MATRIX — openclaw

## Objet

Ce document synthétise la répartition des rôles entre les repos du périmètre.

---

## Matrice

### `opt-trading`
- rôle dominant : exécution / modules durables / compaction `memory_bricks`
- source dominante pour : état réel d’exécution, structure module, closeouts techniques locaux
- ne doit pas devenir : canon transverse unique de gouvernance

### `openclaw`
- rôle dominant : gouvernance transverse / workflow / statuts / séquence GO
- source dominante pour : méthode uniforme, lifecycle, statuts, garde-fous
- ne doit pas devenir : repo d’exécution runtime ou source compacte maîtresse

### `localcms`
- rôle dominant : produit / consumer humain / continuité projet
- source dominante pour : reprises projet, next projet, closeouts projet, consommation de `memory_bricks`
- ne doit pas devenir : canon d’exécution ni canon transverse de gouvernance

### `llm_wiki_minimal`
- rôle dominant : pré-consolidation documentaire
- source dominante pour : fiches minimales, candidats de promotion, clarification intermédiaire
- ne doit pas devenir : mémoire souveraine ni source canonique des closeouts

### `hf_trading`
- rôle actuel : hors périmètre principal immédiat
- règle : sera ouvert ensuite selon la méthode fixée

---

## Règles anti-conflit

### 1. Un rôle dominant par repo

### 2. Un repo dérivé ne remplace pas la source dominante de sa couche

### 3. Le consumer n’absorbe pas le canon

### 4. Le sas de pré-consolidation ne devient pas la mémoire finale

---

## Statut

Statut :
- document transverse de référence
