# RESULT CONTRACT V1

## 1. Objectif

Definir le format canonique minimal des resultats V1 pour OpenClaw Brev-like. Ce contrat unifie les sorties de bootstrap et de smoke dans une seule structure documentaire, sans inventer de runtime reel, de logging complet ni de pipeline d'observabilite.

## 2. Perimetre

Ce contrat couvre :
- le noyau minimal des champs de resultat ;
- les enums autorises ;
- l'unification bootstrap / smoke ;
- les regles minimales de coherence ;
- la representation des restrictions et des refus ;
- la lecture canonique des cas `standard`, `restricted`, `document_only`, `forbidden`.

## 3. Non-perimetre

Ce contrat ne couvre pas :
- l'execution reelle d'un bootstrap ou d'un smoke ;
- le stockage reel des resultats ;
- la journalisation detaillee ;
- les artefacts runtime ;
- les preuves d'etat machine ;
- tout resultat "comme si" le systeme avait tourne.

## 4. Sources du resultat

Un resultat V1 peut provenir uniquement de :
- l'evaluation bootstrap definie dans `bootstrap_contract_v1.md` ;
- l'evaluation smoke definie dans `RUNBOOK_LAUNCHABLE_SMOKE_V1.md` ;
- la validation statique KB-202 ;
- le registry canonique ;
- la spec Launchable et la policy securite.

## 5. Structure canonique du resultat

```yaml
result_version: v1
result_kind: LaunchableEvaluationResult
evaluation_stage: bootstrap
decision: accepted
result_class: standard
result_status: pass
launchable_name: lab-student
registry_id: launchable-lab-student-v1
profile: lab
target_node: student
phase_guard: standard-compute
policy_summary:
  network_mode: loopback-only
  auth_mode: local-user
  sandbox_mode: process
restriction_detected: []
refusal_reasons: []
checkpoint_summary:
  validation_state: VALID
  registry_match: true
  phase_allowed: true
external_preconditions_not_proven: []
```

## 6. Champs obligatoires

Les champs suivants sont obligatoires :
- `result_version`
- `result_kind`
- `evaluation_stage`
- `decision`
- `result_class`
- `result_status`
- `launchable_name`
- `registry_id`
- `profile`
- `target_node`
- `phase_guard`
- `restriction_detected`
- `refusal_reasons`

## 7. Champs optionnels autorises

Les champs optionnels reconnus en V1 sont :
- `policy_summary`
- `checkpoint_summary`
- `external_preconditions_not_proven`
- `notes`

Tout autre champ est hors contrat V1.

## 8. Enums autorises

### `evaluation_stage`
- `bootstrap`
- `smoke`

### `decision`
- `accepted`
- `accepted_with_restriction`
- `refused`
- `not_applicable`

### `result_class`
- `standard`
- `restricted`
- `document_only`
- `forbidden`

### `result_status`
- `pass`
- `restricted_pass`
- `refused`
- `not_applicable`

## 9. Regles d'unification bootstrap / smoke

Le contrat V1 n'autorise pas des paires concurrentes de champs du type :
- `bootstrap_decision` / `smoke_decision`
- `bootstrap_class` / `smoke_class`
- `bootstrap_status` / `smoke_status`

L'unification se fait ainsi :
- `evaluation_stage` porte le contexte : `bootstrap` ou `smoke`
- `decision` porte la decision commune
- `result_class` porte la classe canonique commune
- `result_status` porte l'etat final commun

Les regles de mapping sont :
- `bootstrapable-v1-doc` -> `result_class: standard`
- `bootstrapable-v1-restricted` -> `result_class: restricted`
- `document-only` ou `not-smokable-document-only` -> `result_class: document_only`
- `forbidden` -> `result_class: forbidden`

## 10. Regles de coherence minimales

- `result_version` doit valoir `v1`
- `result_kind` doit valoir `LaunchableEvaluationResult`
- si `decision = refused`, alors `refusal_reasons` ne doit pas etre vide
- si `decision = accepted_with_restriction`, alors `restriction_detected` ne doit pas etre vide
- si `result_class = restricted`, alors `restriction_detected` ne doit pas etre vide
- si `result_class = document_only`, alors `decision` ne peut pas valoir `accepted`
- si `result_class = forbidden`, alors `decision` doit valoir `refused`
- si `result_status = refused`, alors `decision` doit valoir `refused`
- si `result_status = not_applicable`, alors `decision` doit valoir `not_applicable`
- si `evaluation_stage = smoke` et `result_class = document_only`, alors `result_status` doit valoir `not_applicable`
- `launchable_name`, `registry_id`, `profile` et `target_node` doivent etre coherents avec le registry
- `phase_guard` doit etre celui porte par l'entree registry correspondante

## 11. Regles pour `restriction_detected`

- `restriction_detected` est une liste de labels courts ;
- la liste vide est autorisee seulement pour un resultat `standard` ;
- pour un resultat `restricted`, la liste ne doit jamais etre vide ;
- exemples de labels admis :
  - `control-plane-exception`
  - `loopback-only`
  - `restricted-doc`
  - `future-target`
  - `external-preconditions-not-proven`

## 12. Regles pour `refusal_reasons`

- `refusal_reasons` est une liste de messages ou labels bloquants ;
- la liste vide est autorisee seulement si `decision` n'est pas `refused` ;
- elle doit etre renseignee pour tout cas `forbidden` ou `refused` ;
- exemples de motifs admis :
  - `invalid-launchable`
  - `registry-mismatch`
  - `phase-not-allowed`
  - `document-only-case`
  - `db-layer-general-compute-forbidden`
  - `policy-incoherent`

## 13. Cas special `db-layer`

Le contrat impose les regles suivantes :
- si `target_node = db-layer`, alors `result_class` ne peut jamais valoir `standard` ;
- si `target_node = db-layer`, alors le meilleur cas possible est :
  - `decision: accepted_with_restriction`
  - `result_class: restricted`
  - `result_status: restricted_pass`
- si `target_node = db-layer` et `profile != inference`, le resultat doit etre `forbidden` ou `refused` ;
- si `target_node = db-layer`, `restriction_detected` doit contenir au minimum `control-plane-exception` ;
- le resultat `db-layer` doit rester coherent avec :
  - `phase_guard: control-plane-exception-only`
  - `exception_class: inference-local-validation` dans le registry ;
  - une lecture loopback et non generaliste.

## 14. Exemples minimaux de lecture

### Cas standard admissible
- `evaluation_stage: bootstrap`
- `decision: accepted`
- `result_class: standard`
- `result_status: pass`
- `launchable_name: lab-student`
- `target_node: student`

### Cas restreint admissible
- `evaluation_stage: smoke`
- `decision: accepted_with_restriction`
- `result_class: restricted`
- `result_status: restricted_pass`
- `launchable_name: inference-db-layer`
- `target_node: db-layer`
- `restriction_detected: [control-plane-exception, loopback-only]`

### Cas non applicable
- `evaluation_stage: smoke`
- `decision: not_applicable`
- `result_class: document_only`
- `result_status: not_applicable`
- `launchable_name: training-cloud-future`
- `target_node: cloud-gpu-future`

### Cas refuse
- `evaluation_stage: bootstrap`
- `decision: refused`
- `result_class: forbidden`
- `result_status: refused`
- `refusal_reasons: [db-layer-general-compute-forbidden]`

## 15. Point de raccord futur vers logs / artefacts

Un contrat ulterieur de logs ou d'artefacts pourra ajouter autour de ce noyau :
- horodatage ;
- identifiant d'evaluation ;
- emplacement de sortie ;
- references de documents annexes.

Ces extensions ne doivent pas modifier les enums ni les champs obligatoires du noyau V1.
