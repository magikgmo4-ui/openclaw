# SESSION OPENING INDEX OPENCLAW V1

## 1. But du document

Donner l index minimal d ouverture de session OpenClaw.
Ce document dit quoi lire, dans quel ordre, selon quel type de session, avec quel point de depart minimal, sans relire tout l historique et sans rouvrir des chantiers clos.

## 2. Point d entree minimal

Point d entree minimum pour tout operateur, y compris nouveau :

1. `WORKFLOW_OPENCLAW.md`
2. `REPO_BOUNDARIES_OPENCLAW.md`
3. `ETABLI_OPENCLAW.md`

Lecture minimale attendue apres ces 3 documents :

- OpenClaw reste doc/gouvernance d abord.
- `db-layer` reste control plane uniquement.
- `student` est le compute local standard deja qualifie.
- `cloud-gpu-future` reste une cible documentaire future.
- Aucun repo GitHub dedie n est encore a creer tant que la revalidation n est pas prononcee.

Sans cette base, aucune session ne doit partir vers runtime, service, repo ou nouveau cadrage.

## 3. Ordre de lecture recommande

### Ordre minimal standard

1. `WORKFLOW_OPENCLAW.md`
2. `REPO_BOUNDARIES_OPENCLAW.md`
3. `ETABLI_OPENCLAW.md`

### Ordre minimal recommande pour reprise generale

1. `WORKFLOW_OPENCLAW.md`
2. `REPO_BOUNDARIES_OPENCLAW.md`
3. `ETABLI_OPENCLAW.md`
4. `PHASE2_CLOSEOUT_CANONICAL_V1.md`
5. `OPENCLAW_PLAN_KANBAN_CANONIQUE_V1.md`

Lecture courte : d abord les regles, ensuite les frontieres, ensuite l etat acquis, ensuite le point de cloture du socle, puis la carte des chantiers.

## 4. Table des documents essentiels avec role

| Document | Role operatoire | Statut de lecture |
|---|---|---|
| `WORKFLOW_OPENCLAW.md` | regles de session, priorite des sources, classification des travaux, conditions de close | OBLIGATOIRE |
| `REPO_BOUNDARIES_OPENCLAW.md` | frontieres du futur repo, separation doc/runtime, separation control plane / compute plane | OBLIGATOIRE |
| `ETABLI_OPENCLAW.md` | etat acquis reel, machines, elements etablis, limites, point de lecture minimal | OBLIGATOIRE |
| `PHASE2_CLOSEOUT_CANONICAL_V1.md` | point de cloture du socle documentaire reutilisable | CONTEXTUEL RECOMMANDE |
| `OPENCLAW_PLAN_KANBAN_CANONIQUE_V1.md` | carte des KB, ordre global des chantiers, statut de passe | CONTEXTUEL RECOMMANDE |
| `OPENCLAW_BREV_LIKE_MASTER_PLAN_V1.md` | vision cible et non-objectifs de cadrage | CONTEXTUEL |
| `SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md` | format canonique des launchables | CONTEXTUEL |
| `launchables_registry.yaml` | index canonique des launchables et des roles node | CONTEXTUEL |
| `VALIDATION_RULES_LAUNCHABLES_V1.md` | regles statiques de validation | CONTEXTUEL |
| `VALIDATION_MATRIX_LAUNCHABLES_V1.md` | matrice rapide des cas `VALID` / `VALID_WITH_RESTRICTION` / `INVALID` | CONTEXTUEL |
| `INFRA_ROLE_MAPPING_OPENCLAW_V1.md` | roles machines et non-melange infra | CONTEXTUEL |
| `SECURITY_POLICY_OPENCLAW_BREV_V1.md` | garde-fous reseau, auth, sandbox, exposition | CONTEXTUEL |
| `RUNBOOK_LAUNCHABLE_SMOKE_V1.md` | lecture smoke documentaire V1 | CONTEXTUEL |
| `KB301_STUDENT_NODE_INTEGRATION_RUNBOOK_V1.md` | canon de qualification du node `student` | CONTEXTUEL SPECIALISE |
| `KB301_STUDENT_REAL_QUALIFICATION_REPORT_V1.md` | preuve que `student` est deja integre | CONTEXTUEL SPECIALISE |
| `KB302_EXPOSABLE_SERVICES_POLICY_V1.md` | canon des services exposables sur `student` | CONTEXTUEL SPECIALISE |
| `KB302_LAB_JUPYTER_CLASSIFICATION_REPORT_V1.md` | classification du cas `lab` + `jupyter` loopback | CONTEXTUEL SPECIALISE |
| `KB302_LAB_JUPYTER_LOOPBACK_RUNBOOK_V1.md` | runbook local borne pour le seul cas service deja cadre | CONTEXTUEL SPECIALISE |
| `OPENCLAW_REPO_DECISION_REPORT_V1.md` | etat de decision sur la creation du repo dedie | CONTEXTUEL SPECIALISE |

## 5. Parcours par type de session

### Reprise generale

Lire dans cet ordre :

1. `WORKFLOW_OPENCLAW.md`
2. `REPO_BOUNDARIES_OPENCLAW.md`
3. `ETABLI_OPENCLAW.md`
4. `PHASE2_CLOSEOUT_CANONICAL_V1.md`
5. `OPENCLAW_PLAN_KANBAN_CANONIQUE_V1.md`

But : repartir du bon niveau, retrouver le socle clos, voir ce qui reste ouvert sans relecture integrale.

### Cadrage

Lire dans cet ordre :

1. `WORKFLOW_OPENCLAW.md`
2. `REPO_BOUNDARIES_OPENCLAW.md`
3. `ETABLI_OPENCLAW.md`
4. `OPENCLAW_BREV_LIKE_MASTER_PLAN_V1.md`
5. `SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md`
6. `INFRA_ROLE_MAPPING_OPENCLAW_V1.md`
7. `SECURITY_POLICY_OPENCLAW_BREV_V1.md`

But : cadrer sans sortir du modele Brev-like ni melanger control plane et compute.

### Qualification node

Lire dans cet ordre :

1. `WORKFLOW_OPENCLAW.md`
2. `REPO_BOUNDARIES_OPENCLAW.md`
3. `ETABLI_OPENCLAW.md`
4. `launchables_registry.yaml`
5. `VALIDATION_RULES_LAUNCHABLES_V1.md`
6. `KB301_STUDENT_NODE_INTEGRATION_RUNBOOK_V1.md`
7. `KB301_STUDENT_REAL_QUALIFICATION_REPORT_V1.md`

Lecture operative : KB-301 est deja `DONE`. Ne pas le rouvrir par reflexe. Le relire sert a comprendre la preuve canonique et a verifier une question precise sur `student`, pas a relancer le chantier.

### Policy service

Lire dans cet ordre :

1. `WORKFLOW_OPENCLAW.md`
2. `REPO_BOUNDARIES_OPENCLAW.md`
3. `ETABLI_OPENCLAW.md`
4. `SECURITY_POLICY_OPENCLAW_BREV_V1.md`
5. `KB302_EXPOSABLE_SERVICES_POLICY_V1.md`
6. `KB302_LAB_JUPYTER_CLASSIFICATION_REPORT_V1.md`

Lecture operative : KB-302 policy est deja `DONE`. On l utilise pour classer un besoin de service, pas pour reouvrir le chantier.

### Runbook local

Lire dans cet ordre :

1. `WORKFLOW_OPENCLAW.md`
2. `REPO_BOUNDARIES_OPENCLAW.md`
3. `ETABLI_OPENCLAW.md`
4. `KB302_EXPOSABLE_SERVICES_POLICY_V1.md`
5. `KB302_LAB_JUPYTER_LOOPBACK_RUNBOOK_V1.md`

Lecture operative : ne partir en runbook que si le cas est deja classe et borne. En V1, le seul cas explicitement cadre ici est `lab` + `jupyter` loopback sur `student`.

### Decision repo

Lire dans cet ordre :

1. `WORKFLOW_OPENCLAW.md`
2. `REPO_BOUNDARIES_OPENCLAW.md`
3. `ETABLI_OPENCLAW.md`
4. `SESSION_OPENING_INDEX.md`
5. `OPENCLAW_REPO_DECISION_REPORT_V1.md`

Lecture operative : la decision actuelle reste negative tant que la revalidation n est pas faite. Ne pas creer le repo GitHub par reflexe documentaire.

## 6. Documents obligatoires vs contextuels

### Obligatoires pour toute session

- `WORKFLOW_OPENCLAW.md`
- `REPO_BOUNDARIES_OPENCLAW.md`
- `ETABLI_OPENCLAW.md`

### Contextuels recommandes selon besoin

- `PHASE2_CLOSEOUT_CANONICAL_V1.md`
- `OPENCLAW_PLAN_KANBAN_CANONIQUE_V1.md`
- `OPENCLAW_BREV_LIKE_MASTER_PLAN_V1.md`
- `SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md`
- `launchables_registry.yaml`
- `VALIDATION_RULES_LAUNCHABLES_V1.md`
- `VALIDATION_MATRIX_LAUNCHABLES_V1.md`
- `INFRA_ROLE_MAPPING_OPENCLAW_V1.md`
- `SECURITY_POLICY_OPENCLAW_BREV_V1.md`
- `RUNBOOK_LAUNCHABLE_SMOKE_V1.md`

### Specialises

- `KB301_STUDENT_NODE_INTEGRATION_RUNBOOK_V1.md`
- `KB301_STUDENT_REAL_QUALIFICATION_REPORT_V1.md`
- `KB302_EXPOSABLE_SERVICES_POLICY_V1.md`
- `KB302_LAB_JUPYTER_CLASSIFICATION_REPORT_V1.md`
- `KB302_LAB_JUPYTER_LOOPBACK_RUNBOOK_V1.md`
- `OPENCLAW_REPO_DECISION_REPORT_V1.md`

## 7. Regles anti-erreur d ouverture

- Toujours commencer par `WORKFLOW_OPENCLAW.md`, `REPO_BOUNDARIES_OPENCLAW.md`, `ETABLI_OPENCLAW.md` avant de descendre dans un sujet specialise.
- Ne pas repartir du niveau spec si la question porte seulement sur un verdict acquis, un runbook borne, ou une decision deja documentee.
- Ne pas rouvrir KB-301 ni KB-302 pour simple relecture : ces chantiers servent ici de references closes, sauf decision explicite d ouvrir un nouveau chantier distinct.
- Ne jamais convertir une difficulte sur `student` en fallback pratique sur `db-layer`.
- Ne jamais lire `cloud-gpu-future` comme cible runtime active.
- Ne jamais traiter un document contextuel comme autorisation implicite de runtime reel.
- Ne jamais creer le repo GitHub dedie tant que la revalidation documentaire n a pas explicitement change le verdict.
- Si le besoin ne rentre pas clairement dans un des parcours ci-dessus, revenir au parcours `reprise generale` plutot que d improviser un nouveau point d entree.

## 8. Condition de mise a jour du document

Mettre a jour ce document seulement si l un des cas suivants arrive :

- un nouveau document canonique change l ordre minimal de lecture ;
- un nouveau type de session OpenClaw devient canonique ;
- un chantier clos change le point d entree operatoire ;
- la decision sur le repo dedie change apres revalidation explicite.

Ne pas le mettre a jour pour ajouter de l historique detaille, des notes de session, ou un journal de travail.
