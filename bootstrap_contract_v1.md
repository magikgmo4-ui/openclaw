# BOOTSTRAP CONTRACT V1

## 1. Objectif

Definir le contrat minimal de bootstrap V1 pour OpenClaw Brev-like, sans lancer de runtime. Ce contrat indique quand un Launchable peut entrer en bootstrap documentaire, quelles verifications doivent reussir, ce qui peut etre prepare conceptuellement, ce qui doit etre refuse, et quelles sorties minimales doivent etre produites.

## 2. Perimetre

Ce contrat couvre :
- l'eligibilite d'un Launchable au bootstrap V1 ;
- les prerequis documentaires et statiques ;
- la classification bootstrapable ou non ;
- la sequence conceptuelle minimale ;
- les refus bloquants ;
- le cas special `db-layer` ;
- les sorties minimales attendues.

## 3. Non-perimetre

Ce contrat ne couvre pas :
- l'execution reelle d'un bootstrap ;
- les scripts shell, runners ou orchestrateurs ;
- le demarrage de services ;
- la gestion detaillee des secrets ;
- le scheduling, l'autoscaling ou le lifecycle avance ;
- le contrat detaille des logs ;
- le smoke runbook detaille.

## 4. Objets d'entree

Le bootstrap V1 ne peut raisonner qu'a partir des entrees suivantes :
- un Launchable YAML valide statiquement ;
- une entree coherente dans `launchables_registry.yaml` ;
- un node reference present dans `nodes` du registry ;
- une policy reseau/auth coherente avec la spec et KB-202 ;
- une phase autorisee via `allowed_in_phase`.

Des preconditions externes peuvent etre mentionnees comme non prouvees, mais elles ne doivent jamais etre elevees au rang de verite runtime.

## 5. Prerequis obligatoires

Avant tout bootstrap V1, les conditions suivantes doivent etre vraies :
- le Launchable est classe `VALID` ou `VALID_WITH_RESTRICTION` par les regles KB-202 ;
- le fichier source correspond exactement a une entree de registry ;
- `metadata.name`, `spec.profile`, `spec.node.target` et `source` sont coherents avec le registry ;
- le node cible porte le role attendu dans le registry ;
- `spec.policy.exposure.defaultDeny` vaut `true` ;
- tout service actif respecte les contraintes de bind, visibility et auth ;
- `phase-2-early` est present dans `allowed_in_phase`.

Si une seule de ces conditions manque, le bootstrap doit etre refuse.

## 6. Conditions d'eligibilite au bootstrap

Un Launchable est eligible au bootstrap V1 s'il satisfait simultanement :
- validation statique reussie ;
- coherence Launchable -> Registry reussie ;
- node cible autorise pour le profil ;
- classe bootstrap autorisee par ce contrat ;
- aucune violation de doctrine infra ou de policy reseau.

L'eligibilite ne signifie pas readiness runtime. Elle signifie seulement : autorise a produire un plan de bootstrap conceptuel V1.

## 7. Classification par profil / node

### Classes autorisees
- `bootstrapable-v1-doc` : bootstrap documentaire standard autorise ; mappe vers `result_class: standard`
- `bootstrapable-v1-restricted` : bootstrap documentaire autorise mais restreint ; mappe vers `result_class: restricted`
- `document_only` : description admissible, non bootstrapable en V1 documentaire standard ; mappe vers `result_class: document_only`
- `forbidden` : refus obligatoire ; mappe vers `result_class: forbidden`

### Table canonique

| Profil | Node | Classe | Regle |
|---|---|---|---|
| `lab` | `student` | `bootstrapable-v1-doc` | compute local standard |
| `vision` | `student` | `bootstrapable-v1-doc` | compute local standard |
| `hf_build` | `student` | `bootstrapable-v1-doc` | compute local standard |
| `training` | `student` | `bootstrapable-v1-doc` | autorise si present au registry et statiquement valide |
| `training` | `cloud-gpu-future` | `document_only` | cible future, pas de bootstrap standard avant preuves externes |
| `inference` | `cloud-gpu-future` | `document_only` | cible future, pas de bootstrap standard avant preconditions cloud |
| `inference` | `db-layer` | `bootstrapable-v1-restricted` | unique exception control-plane |
| autre combinaison | tout node | `forbidden` | refus obligatoire |

## 8. Profils bootstrapables en V1 documentaire

Sont bootstrapables en V1 documentaire :
- `lab` sur `student`
- `vision` sur `student`
- `hf_build` sur `student`
- `training` sur `student` si un Launchable valide de ce type existe et est enregistre
- `inference` sur `db-layer` uniquement comme exception restreinte

## 9. Profils documentaires seulement

Restent `document_only` en V1 :
- `training` sur `cloud-gpu-future`
- `inference` sur `cloud-gpu-future`

Raison : absence de preuves externes minimales sur la cible cloud GPU et politique de phase qui maintient ces cas en preparatoire.

## 10. Etapes conceptuelles de bootstrap

Le bootstrap V1 suit la sequence logique suivante :

1. Charger les artefacts documentaires
- charger le Launchable cible ;
- charger `launchables_registry.yaml` ;
- charger les regles de validation V1.

2. Verifier la validite documentaire
- verifier la structure du Launchable ;
- verifier la coherence avec le registry ;
- verifier la compatibilite profil/node ;
- verifier les contraintes de policy.

3. Determiner la classe de bootstrap
- resoudre la classe parmi `bootstrapable-v1-doc`, `bootstrapable-v1-restricted`, `document_only`, `forbidden` ;
- resoudre le role du node cible et la garde de phase.

4. Produire un plan conceptuel de bootstrap
- identifier le node cible ;
- identifier le workspace cible ;
- identifier le preset de bootstrap si present ;
- resumer les restrictions de policy applicables ;
- resumer les preconditions externes non prouvees.

5. Emettre une decision
- accepter en mode documentaire ;
- accepter avec restriction ;
- refuser.

## 11. Ce qu'un bootstrap peut preparer

Sans rien executer, un bootstrap V1 peut preparer :
- une decision de bootstrap ;
- une classe de bootstrap ;
- un recapitulatif du profil, du node et du workspace ;
- un resume de la policy applicable ;
- une liste de restrictions ;
- une liste de preconditions externes non prouvees ;
- un statut de resultat.

## 12. Ce qu'un bootstrap doit strictement refuser

Le bootstrap doit refuser si :
- le Launchable est `INVALID` selon KB-202 ;
- aucune entree de registry ne correspond au fichier source ;
- le profil et le node sont incompatibles ;
- la phase n'autorise pas le cas ;
- le node cible est `db-layer` sans satisfaire l'exception canonique ;
- un service actif viole la policy loopback/auth ;
- un compute generaliste est tente sur `db-layer` ;
- la classe resolue est `forbidden` ;
- la classe resolue est `document_only` et qu'un bootstrap standard est demande.

## 13. Cas special `db-layer`

### Regle de base
Un bootstrap normal vers `db-layer` est interdit.

### Seule exception admise
Le seul cas admissible est :
- `profile: inference`
- `node: db-layer`
- entree registry `status: restricted`
- `phase_guard: control-plane-exception-only`
- `exception_class: inference-local-validation`
- `runtime_scope: local-validation-only`
- `sensitivity: sensitive`
- `spec.policy.network.mode: loopback-only`
- tous les services actifs en `loopback`
- aucun GPU alloue

### Lecture obligatoire
Ce cas ne constitue jamais un bootstrap compute standard.
Il doit etre lu comme une exception documentaire restreinte de validation locale sur control plane.

## 14. Sorties minimales attendues

Un futur bootstrap doit produire au minimum un resultat structure contenant :
- `evaluation_stage: bootstrap`
- `decision` : `accepted`, `accepted_with_restriction`, `refused`, `not_applicable`
- `result_class` : `standard`, `restricted`, `document_only`, `forbidden`
- `result_status` : `pass`, `restricted_pass`, `refused`, `not_applicable`
- `launchable_name`
- `profile`
- `target_node`
- `registry_id`
- `phase_guard`
- `restriction_detected` : liste vide ou liste courte
- `policy_summary` : reseau, auth, sandbox
- `refusal_reasons` : liste vide ou renseignee
- `external_preconditions_not_proven` : liste courte si applicable

## 15. Statut de resultat attendu

Le resultat attendu d'un bootstrap V1 suit le canon KB-205 :
- `decision: accepted` + `result_class: standard` + `result_status: pass`
- `decision: accepted_with_restriction` + `result_class: restricted` + `result_status: restricted_pass`
- `decision: not_applicable` + `result_class: document_only` + `result_status: not_applicable`
- `decision: refused` + `result_class: forbidden` + `result_status: refused`

## 16. Raccord vers le futur smoke runbook

Le futur `RUNBOOK_LAUNCHABLE_SMOKE_V1.md` devra consommer ce contrat pour :
- choisir quels cas peuvent etre testes ;
- distinguer les cas standard des cas restreints ;
- interdire tout smoke normal sur `db-layer` hors exception canonique ;
- ignorer les cas `document_only` tant que les preconditions externes ne sont pas etablies.

## 17. Raccord vers le futur results/logs contract

Le futur `RESULT_CONTRACT_V1.md` devra reprendre au minimum :
- `evaluation_stage`
- `decision`
- `result_class`
- `result_status`
- `launchable_name`
- `profile`
- `target_node`
- `restriction_detected`
- `refusal_reasons`

Ce document ne definit pas encore le format complet des artefacts, seulement le noyau minimal a reprendre.
