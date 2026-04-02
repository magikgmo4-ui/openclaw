# OPENCLAW REPO DECISION REPORT V1

## 1. etat de depart repris

- Acquis retenue:
  - phase 2 documentaire socle = CLOSE
  - KB-301 = DONE (student integre)
  - KB-302 policy = DONE (services exposables cadres)
  - socle canonique: spec, registry, validation, bootstrap, smoke, result contracts
  - localcms et opt-trading ont deja leur workflow documentaire aligne
- Ce qui n existe pas encore:
  - aucun repo GitHub dedie openclaw
  - aucune branche openclaw explicite dans opt-trading
  - pas de WORKFLOW_OPENCLAW.md
  - pas de SESSION_OPENING_INDEX.md
  - pas de documentation structurellement alignee sur le modele localcms / opt-trading
- Doctrine de separation:
  - opt-trading reste le repo canonique sur sot/mainline
  - OpenClaw ne doit pas etre melange dans opt-trading sans decision explicite
  - la decision doit etreclare et documentee

## 2. Evaluation de maturite

### Points forts reels

- spec Launchable V1 complete (SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md)
- registry coherence (launchables_registry.yaml)
- validation regles et matrix presente
- bootstrap contract documente
- smoke runbook documente
- result contract documente
- KB-301: node student integre avec preuves reelles
- KB-302: policy services exposables classee
- 12+ documents canoniques produits

### Manques reels

- pas de WORKFLOW_OPENCLAW.md aligne sur localcms/opt-trading
- pas de SESSION_OPENING_INDEX.md documentant les sessions
- pas de REPO_BOUNDARIES_OPENCLAW.md delimitant le perimetre
- pas de ETABLI specifique OpenClaw
- pas de governance explicite du repo separation
- pas de runbook d installation/demarrage pour un nouvel operateur

### Analyse

Le socle documentaire de SPECIFICATION est suffisant pour justifier un repo.
Il manque les documents de GOVERNANCE et D-EXECUTION pour que le repo soit directement utilisable par un nouvel operateur sans reliance sur opt-trading.

Conclusion maturite: 70% precondition atteinte sauf gouvernance et documentation operative.

## 3. Perimetre initial recommande du repo

### Ce qui devrait entrer dans V1

- SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md
- launchables_registry.yaml (avec examples/)
- VALIDATION_RULES_LAUNCHABLES_V1.md
- VALIDATION_MATRIX_LAUNCHABLES_V1.md
- bootstrap_contract_v1.md
- RUNBOOK_LAUNCHABLE_SMOKE_V1.md
- RESULT_CONTRACT_V1.md
- INFRA_ROLE_MAPPING_OPENCLAW_V1.md
- SECURITY_POLICY_OPENCLAW_BREV_V1.md
- OPENCLAW_PLAN_KANBAN_CANONIQUE_V1.md
- KB301_STUDENT_NODE_INTEGRATION_RUNBOOK_V1.md
- KB302_EXPOSABLE_SERVICES_POLICY_V1.md
- KB302_LAB_JUPYTER_CLASSIFICATION_REPORT_V1.md
- KB302_LAB_JUPYTER_LOOPBACK_RUNBOOK_V1.md
- [NEW] WORKFLOW_OPENCLAW.md
- [NEW] REPO_BOUNDARIES_OPENCLAW.md

### Ce qui doit rester hors perimetre initial

- scripts runtime d execution reels
- images container ou artifacts build
- jetons cloud GPU
- dependances tierces non documentees
- tunes de prod non documentees
- historique de migration depuis opt-trading

## 4. Socle documentaire minimal recommande

Le socle doit contenir:

| Document | Role |
|---|---|
| WORKFLOW_OPENCLAW.md | governance du repo, regles de contribution, cycle de vie |
| REPO_BOUNDARIES_OPENCLAW.md | delimitation entre control plane / compute plane |
| ETABLI_OPENCLAW.md | liste des assets, tokens, machines, runtimes |
| SESSION_OPENING_INDEX.md | index des sessions de travail |
| KANBAN_OPENCLAW.md | etat courant des chantiers |
| README.md | point entree rapide |

Ces documents ne sont pas encore alignes sur le workflow localcms / opt-trading.

## 5. Verdict de decision

`repo_dedie_pas_encore_recommande`

La matiere speculative est presente et justifie un repo separe, mais il manque les documents de gouvernance et d execution operatoire necessaires pour qu un nouvel operateur puisse demarrer avec OpenClaw sans se referer constamment a opt-trading ou a une doc externe.

La creation du repo ne doit pas etre declenchee tant que ces conditions ne sont pas satisfaites.

## 6. Conditions de declenchement

Si repo dedie recommande:

- WORKFLOW_OPENCLAW.md doit exister et etre approuve
- REPO_BOUNDARIES_OPENCLAW.md doit exister et etre approuve
- ETABLI_OPENCLAW.md doit exister et documenter les machines, tokens, runtimes des nodes
- SESSION_OPENING_INDEX.md doit exister et lister les sessions existantes
- Un runbook de demarrage minimal doit permettre a un nouvel operateur d utiliser OpenClaw sans doc externe

La creation effective du repo ne doit pas etre lancee avant ces conditions.

Plus petit manque a combler:

- WORKFLOW_OPENCLAW.md aligne sur le modele localcms / opt-trading

## 7. Point de reprise suivant

Plus petit prochain pas utile:

- Produire WORKFLOW_OPENCLAW.md selon le meme format que localcms
- Puis produire REPO_BOUNDARIES_OPENCLAW.md, ETABLI_OPENCLAW.md et SESSION_OPENING_INDEX.md
- Une fois ces 4 documents produits, re-evaluer pour eventuellement declarer repo dedie recommandable

Ne pas creer le repo GitHub avant que ces conditions soient satisfaites.