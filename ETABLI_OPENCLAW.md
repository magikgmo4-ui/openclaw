# ETABLI OPENCLAW V1

## 1. But du document

Documenter l etat etabli minimal d OpenClaw sans exposer de secrets. Ce document indique ce qui est reellement etabli, ce qui ne l est pas, et les limites a respecter pour eviter une lecture erronee de maturite.

## 2. Perimetre de l etabli

Ce document decrit:
- Les machines et roles retenus comme canoniques
- Les elements documentaires reellement produits
- Les elements non etablis ou futurs
- Les limites operatoires a respecter

Ce document ne contient pas:
- Secrets, jetons, credentials ou mots de passe
- Configurations runtime effectives
- Scripts d execution
- Valeurs sensibles de production

## 3. Machines et roles canoniques

| Machine | Role | Statut | Certitude |
|---|---|---|---|
| db-layer | Control plane OpenClaw | ETABLI | haute |
| student | Compute local standard | ETABLI | haute |
| cloud-gpu-future | Compute GPU externe | NON ACTIF | future |

Role db-layer = controle uniquement. Pas de compute generaliste.
Role student = compute local standard par defaut.
Role cloud-gpu-future = cible future documentaire, pas d activation runtime.

## 4. Elements etablis

### 4.1 Documentation canonique

- SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md
- launchables_registry.yaml
- VALIDATION_RULES_LAUNCHABLES_V1.md
- VALIDATION_MATRIX_LAUNCHABLES_V1.md
- bootstrap_contract_v1.md
- RUNBOOK_LAUNCHABLE_SMOKE_V1.md
- RESULT_CONTRACT_V1.md
- INFRA_ROLE_MAPPING_OPENCLAW_V1.md
- SECURITY_POLICY_OPENCLAW_BREV_V1.md
- PHASE2_CLOSEOUT_CANONICAL_V1.md

### 4.2 Runbooks et classifications

- KB301_STUDENT_NODE_INTEGRATION_RUNBOOK_V1.md
- KB301_PRE_RETEST_CORRECTIVE_REPORT_V1.md
- KB301_STUDENT_REAL_QUALIFICATION_REPORT_V1.md
- KB302_EXPOSABLE_SERVICES_POLICY_V1.md
- KB302_LAB_JUPYTER_CLASSIFICATION_REPORT_V1.md
- KB302_LAB_JUPYTER_LOOPBACK_RUNBOOK_V1.md

### 4.3 Gouvernance

- WORKFLOW_OPENCLAW.md
- REPO_BOUNDARIES_OPENCLAW.md

### 4.4 UI operateur bornee

- KB303_MINIMAL_MOCKUP_V1.html
- Premiere surface operateur locale, statique, lecture seule, sans backend ni reseau

### 4.5 Exemples

- examples/launchable_lab_student.yaml
- examples/launchable_vision_student.yaml
- examples/launchable_hf_build_student.yaml
- examples/launchable_training_cloud_future.yaml
- examples/launchable_inference_db_layer.yaml

## 5. Elements NON etablis / futurs

### 5.1 Non etablis

- Runtime execution effectif sur student (au-dela du probe hf_build)
- Pipeline de logs complet
- Dashboard operateur large
- Integrations cloud GPU

### 5.2 Futurs documentes mais non actifs

- cloud-gpu-future: aucun runtime actif
- localcms: pas de dependance avec OpenClaw
- opt-trading: separation maintenue
- Repo GitHub dedie: conditionnee a 4 documents de gouvernance produits

## 6. Runtimes et execution

| Element | Statut | Preuve |
|---|---|---|
| podman sur student | ETABLI PROBE | run --rm alpine true fonctionne |
| docker sur student | NON TESTE | absent ou non utilise |
| jupyter lab sur student | ETABLI BORNE | ouverture locale prouvee sur `lab`, bind `127.0.0.1`, port `8888`, auth `token`, sandbox `process`, sans LAN/public |
| hf_build execution physique | DEPENDANT IMAGE | image declaree absente |
| vision GPU compute | NON PROUVE | pas de nvidia-smi |

Le runtime container est preuve fonctionnel uniquement avec podman. Une execution applicative locale bornee est maintenant prouvee pour le seul cas `lab` + `jupyter` sur `student`, en loopback strict, sans generaliser la maturite runtime globale.

## 7. Limites explicites

- Pas de credential dans la doc
- Pas de JWT production
- Pas de wallet cles ou tokens
- Pas de configurations reseau non documentees
- Pas de DB access creds dans les launchables

Aucune valeur sensible ne doit etre ajoutee a ces documents. Si une valeur doit apparaitre, elle doit rester dans un referentiel externe non documente.

## 8. Regles de non-divulgation

- AUCUN secret, token ou credential ne doit apparaitre dans les documents
- Si une valeur sensible doit etre utilisee, elle reste en memoire de l operateur ou dans un vault externe
- Les logs d execution sont exclus de la documentation permanente
- Les configurations specifiques machine restent hors de la documentation canonique
- Les scripts d installation temporaires sont a effacer apres usage

Tout document qui expose un secret est invalide et doit etre corrige.

## 9. Condition de mise a jour

Ce document doit etre mis a jour quand:

- Un nouveau KB est cloture
- Un nouveau role machine est integre
- Un runtime passe de non etabli a etabli avec preuve
- Une limite doctrinale change

Ce document ne doit pas etre mis a jour pour ajouter des secrets ou credentials.

## 10. Point de lecture minimal

Un nouvel operateur doit savoir:

1. OpenClaw = control plane Brev-like. Compute sur student.
2. db-layer = control plane uniquement. Pas de compute generaliste.
3. student = compute local standard. Premiere activation reelle connue.
4. cloud-gpu-future = cible future. Pas d activation runtime.
5. La documentation canonique est complete. L execution effective est limitee.
6. Pour demarrer une session: WORKFLOW puis REPO BOUNDARIES.
7. Pour lancer un service: voir KB302 Loopback Runbook.
8. Pour toute question: voir etablis ci-dessus.

Cet etat est le POINT DE DEPART minimum avant toute session OpenClaw.
