---
doc_id: OPENCLAW_UNIFORM_CONTINUITY_METHOD
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
  - workflow
  - continuity
  - method
surface: governance
source_kind: canonical
updated_at: 2026-04-11
links: []
---

# UNIFORM_CONTINUITY_METHOD — openclaw

## Objet

Ce document fixe la méthode uniforme de continuité au niveau transverse.

Il sert à :
- clarifier les rôles des repos
- clarifier les couches documentaires
- réduire les conflits de source de vérité
- garder une continuité exploitable entre chantiers, closeouts, reprises, compaction et pré-consolidation

---

## 1. Rôles des repos

### `opt-trading`
Rôle principal :
- repo canonique principal
- canon d’exécution
- canon structurel des modules durables
- source operative de `memory_bricks`

### `openclaw`
Rôle principal :
- canon gouvernance / workflow / statuts / séquence GO

### `localcms`
Rôle principal :
- repo produit / consumer humain / continuité projet
- consumer de `memory_bricks`

### `llm_wiki_minimal`
Rôle principal :
- pré-consolidation documentaire

### `hf_trading`
Rôle actuel :
- hors périmètre principal immédiat
- à ouvrir ensuite selon la méthode fixée

---

## 2. Couches documentaires

Les couches documentaires retenues sont :
- gouvernance
- chantier
- continuité
- compaction
- pré-consolidation
- couche humaine

### Principe
Chaque couche a une fonction propre.
Aucune couche dérivée ne remplace silencieusement sa source amont.

---

## 3. Pipeline cible

Pipeline visé :

contexte humain / discussion / journal
-> stabilisation documentaire
-> chantier borné
-> closeout / reprise / next
-> compaction via `memory_bricks`
-> pré-consolidation éventuelle

---

## 4. Principes forts

### 4.1 Pas de vérité concurrente
Un artefact dérivé ne remplace pas sa source amont.

### 4.2 Pas de consumer qui devient source maîtresse
Un repo consumer ne devient pas le canon de l’état réel.

### 4.3 Pas de sas qui devient canon final
La pré-consolidation ne devient pas la mémoire souveraine.

### 4.4 Pas de compaction sans traçabilité
Une forme compacte doit pouvoir remonter vers une source plus détaillée.

### 4.5 Réalité avant hypothèse
L’état réel des repos, branches, artefacts et chantiers prime sur la mémoire et les hypothèses.

---

## 5. Ordre d’implémentation retenu

1. `opt-trading`
2. `openclaw`
3. `localcms`
4. `llm_wiki_minimal`
5. `hf_trading`

---

## 6. Statut

Statut :
- document transverse de référence
- à maintenir cohérent avec l’état réel des repos du périmètre
