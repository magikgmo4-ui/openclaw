# OPENCLAW — PLAN KANBAN CANONIQUE V1
_Date de cadrage : 2026-03-25 (America/Montreal)_

## 1. But du chantier

Transformer OpenClaw en **control plane Brev-like** pour l’infrastructure actuelle, sans chercher un clone 1:1 de NVIDIA Brev.

### Modèle cible
- **OpenClaw = control plane**
- **student / db-layer / cloud GPU futur = compute plane**
- **Launchable OpenClaw = unité reproductible de lancement**

## 2. Règles de conduite

### Source de vérité
1. état réel validé
2. spec canonique
3. runbooks
4. docs secondaires

### Hors périmètre pour cette passe
- pas de refonte globale runtime
- pas de migration cloud réelle immédiate
- pas de Jupyter/IDE/API partout dès le début
- pas de clone UI complet de Brev
- pas de chantier NIM/serverless maintenant

### Résultat attendu de la phase V1
Avoir un système compréhensible et versionnable où l’on peut définir :
- **quel agent**
- **quel workspace**
- **quel node**
- **quel bootstrap**
- **quelle policy**
- **quels services**
pour un usage donné (`lab`, `training`, `inference`, `vision`, `hf_build`).

---

## 3. Vue Kanban simple

## CLOSE
| ID | Chantier | Résultat |
|---|---|---|
| KC-001 | Socle OpenClaw canonique | runtime canonique + wrapper + runbook stabilisés |
| KC-002 | Skills simples | `routine-health-check-simple` et `maintenance-checklist-local` validés factuellement |
| KC-003 | Écart probe | `wrapper probe` qualifié comme écart bénin intermittent |
| KC-004 | Patch documentaire | runbook mis à jour avec note opératoire sur `probe` |

## NOW
| ID | Chantier | Objectif | Livrable attendu | Fin de tâche = Done si... |
|---|---|---|---|---|
| KB-101 | Cadrage master plan | figer la vision Brev-like OpenClaw | `OPENCLAW_BREV_LIKE_MASTER_PLAN_V1.md` | la cible, le périmètre et les non-objectifs sont écrits clairement |
| KB-102 | Spec Launchable V1 | définir le format canonique d’un Launchable OpenClaw | `SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md` | les champs obligatoires, optionnels et règles de validation sont définis |
| KB-103 | Exemples de profils | créer des exemples simples et lisibles | 3 à 5 exemples YAML (`lab`, `training`, `inference`, `vision`, `hf_build`) | chaque exemple est cohérent avec l’infra réelle |
| KB-104 | Mapping infra | fixer le rôle de chaque machine | `INFRA_ROLE_MAPPING_OPENCLAW_V1.md` | `db-layer`, `student`, `cloud-gpu-future` ont un rôle explicite et non ambigu |
| KB-105 | Doctrine sécurité V1 | figer les garde-fous | `SECURITY_POLICY_OPENCLAW_BREV_V1.md` | boucle locale, auth, sandbox, exposition ports, trust boundaries sont cadrés |

## NEXT
| ID | Chantier | Objectif | Livrable attendu | Dépend de |
|---|---|---|---|---|
| KB-201 | Registry des launchables | créer l’index canonique des launchables | `launchables_registry.yaml` | KB-102, KB-103 |
| KB-202 | Validation statique | valider la forme des launchables avant runtime | schéma + script de validation | KB-102 |
| KB-203 | Bootstrap standard | définir le bootstrap minimal par profil | `bootstrap_contract_v1.md` + templates | KB-102, KB-103 |
| KB-204 | Plan de smoke | définir comment vérifier un launchable sans chantier lourd | `RUNBOOK_LAUNCHABLE_SMOKE_V1.md` | KB-102, KB-202 |
| KB-205 | Journalisation / outputs | définir le contrat d’artefacts et de logs | `RESULT_CONTRACT_V1.md` | KB-102 |

## LATER
| ID | Chantier | Objectif | Livrable attendu |
|---|---|---|---|
| KB-301 | Node student intégré | faire de `student` le premier compute node Brev-like réel | runbook d’intégration node |
| KB-302 | Services exposables | cadrer Jupyter/API/ports/tunnels par profil | policy services v1 |
| KB-303 | UI opérateur légère | ajouter une lecture simple des launchables et de leur état | dashboard minimal |
| KB-304 | Cloud GPU futur | préparer un node GPU externe compatible avec les launchables | plan d’intégration cloud |
| KB-305 | Inference / model serving | ouvrir le chantier endpoints GPU / vLLM / API | spec separate inference |

## BLOCKERS / A SURVEILLER
| ID | Point | Risque | Action |
|---|---|---|---|
| R-01 | confusion OpenClaw = Brev | dérive de périmètre | rappeler que l’objectif est Brev-like, pas clone |
| R-02 | trop d’implémentation trop tôt | dette / dispersion | commencer par docs + spec + exemples |
| R-03 | sécurité floue | exposition dangereuse | figer la policy avant les services |
| R-04 | mélange control plane / compute plane | architecture sale | garder `db-layer` pour contrôle, `student/cloud` pour compute |
| R-05 | launchables trop abstraits | spec inutilisable | imposer exemples concrets dès V1 |

---

## 4. Ordre recommandé d’exécution

### Phase 1 — Cadrage documentaire (obligatoire)
1. KB-101 — Master plan
2. KB-102 — Spec Launchable V1
3. KB-103 — Exemples YAML
4. KB-104 — Mapping infra
5. KB-105 — Doctrine sécurité

### Phase 2 — Socle versionnable
6. KB-201 — Registry launchables
7. KB-202 — Validation statique
8. KB-203 — Bootstrap contract
9. KB-204 — Runbook smoke
10. KB-205 — Result contract

### Phase 3 — Première activation réelle
11. KB-301 — Node student réel
12. KB-302 — Services exposables
13. KB-303 — UI opérateur légère

### Phase 4 — Extension
14. KB-304 — Cloud GPU futur
15. KB-305 — Inference / model serving

---

## 5. Livrables minimums de la passe actuelle

La passe actuelle doit produire au minimum :

1. `OPENCLAW_BREV_LIKE_MASTER_PLAN_V1.md`
2. `SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md`
3. `INFRA_ROLE_MAPPING_OPENCLAW_V1.md`
4. `SECURITY_POLICY_OPENCLAW_BREV_V1.md`
5. `examples/launchable_lab_student.yaml`
6. `examples/launchable_training_cloud_future.yaml`
7. `examples/launchable_inference_db_layer.yaml`
8. `examples/launchable_vision_student.yaml`
9. `examples/launchable_hf_build_student.yaml`

---

## 6. Définition de done de la passe

La passe est considérée **DONE** si :
- la vision Brev-like est figée sans ambiguïté
- le format Launchable V1 est écrit et compréhensible
- les rôles machines sont figés
- les règles de sécurité V1 sont explicites
- au moins 3 exemples de launchables sont fournis
- le tout est livrable à un exécuteur (`opencode`) sans discussion de cadrage supplémentaire

---

## 7. Message PM simple

### Ce qu’on fait maintenant
On écrit le **cadre canonique**.

### Ce qu’on ne fait pas encore
On ne branche pas encore tout le runtime réel, le cloud GPU ou les services lourds.

### Pourquoi
Parce qu’un bon control plane Brev-like doit d’abord avoir :
- une spec claire
- des profils clairs
- des garde-fous clairs
- des exemples concrets
