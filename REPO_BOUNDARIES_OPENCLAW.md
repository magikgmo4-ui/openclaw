# REPO BOUNDARIES OPENCLAW V1

## 1. But du document

Definir les frontieres exactes du futur repo OpenClaw pour eviter tout melange avec opt-trading, localcms ou une future infra cloud. Ce document fixe ce qui doit entrer dans le repo, ce qui doit rester hors, et quelles separations strictes doivent etre respectees.

## 2. Perimetre du repo OpenClaw V1

Le repo OpenClaw V1 contient uniquement la DOCUMENTATION CANONIQUE et la GOUVERNANCE du systeme. Il ne contient pas de code executable, de runtime, ni de configuration de production. Il est la SOURCE DE VERITE DOCUMENTAIRE pour tout operateur desireux de comprendre ou reproduire le systeme.

Le repo ne remplace pas opt-trading. Il ne fait que documenter les concepts, les regles et les limites de l architecture OpenClaw. L execution reelle reste dans l infra ciblee (student, cloud-gpu-future).

## 3. Ce qui entre dans le repo

### 3.1 Documents de gouvernance

- WORKFLOW_OPENCLAW.md
- REPO_BOUNDARIES_OPENCLAW.md
- ETABLI_OPENCLAW.md
- SESSION_OPENING_INDEX.md
- KANBAN_OPENCLAW.md

### 3.2 Documents de spec et validation

- SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md
- launchables_registry.yaml
- VALIDATION_RULES_LAUNCHABLES_V1.md
- VALIDATION_MATRIX_LAUNCHABLES_V1.md
- bootstrap_contract_v1.md
- RUNBOOK_LAUNCHABLE_SMOKE_V1.md
- RESULT_CONTRACT_V1.md

### 3.3 Documents de cadrage

- OPENCLAW_PLAN_KANBAN_CANONIQUE_V1.md
- OPENCLAW_BREV_LIKE_MASTER_PLAN_V1.md
- INFRA_ROLE_MAPPING_OPENCLAW_V1.md
- SECURITY_POLICY_OPENCLAW_BREV_V1.md

### 3.4 Runbooks et rapports de classification

- KB301_STUDENT_NODE_INTEGRATION_RUNBOOK_V1.md
- KB301_PRE_RETEST_CORRECTIVE_REPORT_V1.md
- KB301_STUDENT_REAL_QUALIFICATION_REPORT_V1.md
- KB302_EXPOSABLE_SERVICES_POLICY_V1.md
- KB302_LAB_JUPYTER_CLASSIFICATION_REPORT_V1.md
- KB302_LAB_JUPYTER_LOOPBACK_RUNBOOK_V1.md

### 3.5 Exemples

- examples/launchable_lab_student.yaml
- examples/launchable_vision_student.yaml
- examples/launchable_hf_build_student.yaml
- examples/launchable_training_cloud_future.yaml
- examples/launchable_inference_db_layer.yaml

## 4. Ce qui reste hors repo

Tout ce qui est RUNTIME EFFECTIF ou INSTALLATION SPECIFIQUE est hors repo:

- scripts de demarrage ou d installation
- Dockerfiles ou container builds
- configurations d environment specifiques a une machine
- jetons, credentials, secrets
- logs d execution
- Artefacts de build resultats
- historique de migration depuis opt-trading

## 5. Frontiere control plane / compute plane

### 5.1 Control plane (db-layer)

- Ni hebergee ni executee dans le repo OpenClaw
- Document uniquement: le role de db-layer est decrit dans INFRA_ROLE_MAPPING_OPENCLAW_V1.md
- Toute activation compute sur db-layer est une violation doctrinale documentee

### 5.2 Compute nodes (student, cloud-gpu-future)

- Document uniquement: les roles sont decrits dans INFRA_ROLE_MAPPING_OPENCLAW_V1.md
- Les configurations specifiques a student ou cloud-gpu-future restent dans leurs environments respectifs
- Le repo OpenClaw ne stocke pas de configs runtime de ces nodes

## 6. Frontiere avec opt-trading

opt-trading est le repo canonique sur sot/mainline. Il doit rester SEPARE d OpenClaw.

- opt-trading = code de trading, configs de production, scripts operationnels
- OpenClaw repo = documentation et gouvernance du framework Brev-like
- Aucune confusion: opt-trading ne depend pas d OpenClaw pour fonctionner
- OpenClaw documente un CONCEPT, pas une dependance d opt-trading

Si une separation physique est necessaire (pas de sous-modules opt-trading vers OpenClaw), cette separation doit etre respectee.

## 7. Frontiere avec localcms

localcms est un repo documentaire orthogonal. Il doit rester SEPARE d OpenClaw.

- localcms = documentation d un autre systeme
- OpenClaw = documentation d un framework Brev-like
- Pas de dependance entre eux
- Le workflow documentaire peut etre similaire, mais ce sont des projets distincts

## 8. Frontiere avec cloud-gpu-future

cloud-gpu-future est une CIBLE DOCUMENTAIRE FUTURE, pas une configuration active.

- Pas d activation runtime reelle dans le repo OpenClaw
- Les launchables visant cloud-gpu-future ont le statut FUTURE dans le registry
- Le repo documente le PERIMETRE FUTURE sans le realiser
- Aucune configuration cloud, SSH, secrets ou RBAC n est stockee

## 9. Interdits de melange

- Pas de melange de code runtime avec documentation de gouvernance
- Pas de melange de secrets ou credentials dans la documentation
- Pas de melange de configurations specifiques machine avec specs generiques
- Pas de melange de logs d execution avec documents de reference
- Pas de melange de scripts d installation avec runbooks conceptuels

## 10. Condition de validite des frontieres

Les frontieres sont reputees valides si :

- Aucune configuration runtime effective n est presente dans le repo
- aucune credential ou secret n est stocke
- la separation db-layer / student / cloud-gpu-future est respectee
- opt-trading et localcms ne dependent pas fonctionnellement d OpenClaw
- le repo ne contient que de la documentation et de la gouvernance

Ces frontieres doivent etre respectees avant toute creation effective du repo GitHub.
