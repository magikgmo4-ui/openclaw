# RUNBOOK LAUNCHABLE SMOKE V1

## 1. Objectif

Definir le runbook de smoke V1 pour les Launchables OpenClaw Brev-like, sans execution reelle. Ce runbook fixe quels cas sont smokables, quels cas sont exclus, quelles preconditions doivent etre reunies, quelle sequence conceptuelle doit etre suivie, et quels resultats minimaux doivent etre releves.

## 2. Perimetre

Ce runbook couvre :
- l'eligibilite d'un Launchable au smoke V1 ;
- la classification des cas de smoke ;
- les preconditions documentaires minimales ;
- la sequence conceptuelle de smoke ;
- les checkpoints minimaux ;
- les conditions d'arret ou de refus ;
- le traitement des cas `restricted-doc` ;
- le raccord minimal vers `RESULT_CONTRACT_V1.md`.

## 3. Non-perimetre

Ce runbook ne couvre pas :
- l'execution d'un smoke reel ;
- les scripts, runners ou automatisations ;
- le demarrage d'un agent, d'un workspace ou d'un service ;
- la validation runtime d'un node ;
- la disponibilite reelle du cloud GPU ;
- la collecte finale de logs detaillee.

## 4. Classes de cas de smoke

Les classes ci-dessous sont des classes operatoires locales de smoke. Elles doivent etre mappees vers le canon KB-205 :
- `smokable-v1-standard` -> `result_class: standard`
- `smokable-v1-restricted` -> `result_class: restricted`
- `not-smokable-document-only` -> `result_class: document_only`
- `forbidden` -> `result_class: forbidden`

## 5. Preconditions obligatoires

Avant tout smoke V1, les conditions suivantes doivent etre reunies :
- Launchable classe `VALID` ou `VALID_WITH_RESTRICTION` par KB-202 ;
- coherence Launchable <-> Registry confirmee ;
- classe bootstrap compatible avec `bootstrap_contract_v1.md` ;
- `phase-2-early` present dans `allowed_in_phase` ;
- policy reseau/auth coherente avec la spec et la security policy ;
- aucune violation bloquante KB-202 ;
- aucun cas `document_only` presente comme smokable standard.

Si une seule condition echoue, le smoke est refuse ou arrete immediatement.

## 6. Cas smokables en V1

### `smokable-v1-standard`
- `lab` sur `student`
- `vision` sur `student`
- `hf_build` sur `student`
- `training` sur `student` si un Launchable valide de ce type existe, est enregistre et passe KB-202 / KB-203

### `smokable-v1-restricted`
- `inference` sur `db-layer`, uniquement si toutes les conditions d'exception control-plane sont satisfaites

## 7. Cas exclus ou interdits

### `not-smokable-document-only`
- `training` sur `cloud-gpu-future`
- `inference` sur `cloud-gpu-future`

### `forbidden`
- toute combinaison `profile/node` interdite par KB-202 ;
- tout Launchable `db-layer` hors exception canonique ;
- tout cas avec derive de policy reseau/auth ;
- tout cas dont le registry ne correspond pas au fichier source.

## 8. Table de classification minimale

| Profil | Node | Classe smoke | Lecture operatoire |
|---|---|---|---|
| `lab` | `student` | `smokable-v1-standard` | smoke documentaire standard |
| `vision` | `student` | `smokable-v1-standard` | smoke documentaire standard |
| `hf_build` | `student` | `smokable-v1-standard` | smoke documentaire standard |
| `training` | `student` | `smokable-v1-standard` | smoke documentaire standard si present et valide |
| `training` | `cloud-gpu-future` | `not-smokable-document-only` | cas preparatoire, hors smoke V1 |
| `inference` | `cloud-gpu-future` | `not-smokable-document-only` | cas preparatoire, hors smoke V1 |
| `inference` | `db-layer` | `smokable-v1-restricted` | unique exception control-plane |
| autre combinaison | tout node | `forbidden` | refus obligatoire |

## 9. Sequence conceptuelle du smoke

1. Charger les artefacts
- charger le Launchable ;
- charger `launchables_registry.yaml` ;
- charger les regles KB-202 ;
- charger `bootstrap_contract_v1.md`.

2. Verifier la validite documentaire
- verifier la validite statique ;
- verifier la coherence Launchable <-> Registry ;
- verifier la compatibilite profil/node ;
- verifier la coherence de policy reseau/auth.

3. Determiner la classe de smoke
- resoudre la bootstrap class ;
- traduire cette classe en `smokable-v1-standard`, `smokable-v1-restricted`, `not-smokable-document-only` ou `forbidden`.

4. Preparer le perimetre de smoke documentaire
- relever l'identite du launchable ;
- relever le node cible ;
- relever le profil ;
- relever la garde de phase ;
- relever les restrictions eventuelles.

5. Executer la lecture operatoire conceptuelle
- confirmer qu'aucune hypothese runtime non prouvee n'est promue comme acquise ;
- confirmer que le cas peut etre lu comme standard, restreint, non smokable ou interdit.

6. Relever les checkpoints minimaux
- documenter les checkpoints obligatoires ;
- marquer tout point d'exclusion ou de restriction.

7. Conclure
- `pass`
- `restricted_pass`
- `not_applicable`
- `refused`

## 10. Checkpoints minimaux

Les checkpoints minimaux a relever sont :
- identite du launchable ;
- profil ;
- node cible ;
- statut registry ;
- `phase_guard` ;
- classe bootstrap ;
- classe smoke ;
- conformite reseau/auth ;
- restriction detectee ;
- caractere smokable ou non ;
- raison d'exclusion ou de refus si applicable.

## 11. Conditions d'arret ou de refus

Arret immediat si :
- le Launchable est `INVALID` ;
- le registry ne correspond pas ;
- la phase n'autorise pas le cas ;
- la classe resolue est `not-smokable-document-only` ;
- la classe resolue est `forbidden` ;
- `db-layer` ne satisfait pas toutes les conditions d'exception ;
- la policy reseau/auth est incoherente ;
- une tentative de compute generaliste vise `db-layer`.

Dans ces cas, le smoke conclut `refused` ou `not_applicable` selon la classe du cas.

## 12. Regles pour `restricted-doc`

Un cas `restricted-doc` :
- ne peut jamais etre traite comme cas standard ;
- doit garder une lecture prudente et bornee ;
- doit faire apparaitre explicitement sa restriction dans la conclusion ;
- doit etre arrete au moindre ecart sur le registry, la policy, la phase ou la doctrine infra.

La meilleure conclusion possible pour un cas `restricted-doc` est `restricted_pass`.

## 13. Cas special `db-layer`

### Regle de base
Aucun smoke standard sur `db-layer`.

### Seule exception
Le seul cas smokable sur `db-layer` est :
- `profile: inference`
- `status: restricted`
- `phase_guard: control-plane-exception-only`
- `exception_class: inference-local-validation`
- `runtime_scope: local-validation-only`
- `sensitivity: sensitive`
- `network.mode: loopback-only`
- services actifs en `loopback`
- aucun GPU alloue

### Lecture obligatoire
Ce cas ne donne jamais permission de tester des compute jobs standards sur `db-layer`.
Il doit etre traite comme une verification documentaire restreinte de validation locale sur le control plane.

## 14. Sorties minimales du smoke

Un futur smoke doit produire au minimum :
- `evaluation_stage: smoke`
- `decision` : `accepted`, `accepted_with_restriction`, `refused`, `not_applicable`
- `result_class` : `standard`, `restricted`, `document_only`, `forbidden`
- `result_status` : `pass`, `restricted_pass`, `refused`, `not_applicable`
- `launchable_name`
- `profile`
- `target_node`
- `registry_id`
- `phase_guard`
- `restriction_detected`
- `checkpoint_summary`
- `refusal_reasons`

## 15. Raccord vers `RESULT_CONTRACT_V1.md`

Le futur `RESULT_CONTRACT_V1.md` devra reprendre au minimum :
- `evaluation_stage`
- `decision`
- `result_class`
- `result_status`
- `launchable_name`
- `profile`
- `target_node`
- `phase_guard`
- `restriction_detected`
- `checkpoint_summary`
- `refusal_reasons`

Ce runbook ne definit pas encore le format complet du resultat final. Il fixe seulement le noyau minimal a transmettre.
