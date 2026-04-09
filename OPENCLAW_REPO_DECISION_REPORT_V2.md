# OPENCLAW REPO DECISION REPORT V2

## 1. Etat de depart repris

- le repo GitHub `openclaw` existe deja
- le repo reste doc/gouvernance-only
- `opt-trading` reste le repo canonique de continuite sur `sot/mainline`
- le runtime effectif reste hors du repo `openclaw`
- les docs de gouvernance existent deja : `WORKFLOW_OPENCLAW.md`, `REPO_BOUNDARIES_OPENCLAW.md`, `ETABLI_OPENCLAW.md`, `SESSION_OPENING_INDEX.md`, `KANBAN_OPENCLAW.md`, `README.md`
- `GO_OPENCLAW_EVIDENCE_01` a ete execute reellement sur `db-layer` sous l utilisateur `openclaw`

## 2. Faits reels nouvellement prouves

### 2.1 Evidence executee

Le module de collecte a exporte les preuves courantes dans :

`/home/openclaw/.openclaw/workspace-orchestrateur/docs_evidence/openclaw_current_state/`

Fichiers regeneres :
- `01_doctor_status.txt`
- `02_configure_status.txt`
- `03_doctor_quick.txt`
- `04_workspace_context.txt`
- `90_PROMPT_DOCS.txt`

### 2.2 Gateway loopback

- premier etat observe : gateway loopback ferme avec erreur `1006 abnormal closure`
- etat confirme par le module de pilotage : `SESSION_STATUS=stopped`
- correction appliquee : demarrage du gateway
- etat ensuite prouve :
  - `Gateway Health` = `OK`
  - `Reachable: yes`
  - `Connect: ok`
  - `RPC: ok`
  - `SESSION_STATUS=running`

## 3. Evaluation de maturite actualisee

### Points forts reels

- le repo dedie existe et borne bien un perimetre doc/gouvernance-only
- le socle documentaire canonique existe
- les frontieres avec `opt-trading`, `localcms`, `db-layer`, `student` et `cloud-gpu-future` sont documentees
- une collecte de preuves reelle recente existe maintenant pour OpenClaw
- le gateway loopback sur `db-layer` a une preuve recente de reprise et de fonctionnement local borne

### Limites maintenues

- aucune preuve nouvelle n autorise un runtime large de production
- aucune preuve nouvelle n autorise un serving modele expose
- aucune preuve nouvelle n autorise du compute standard sur `db-layer`
- la maturite runtime reste volontairement limitee et ne doit pas etre surevaluee

## 4. Contradictions documentaires heritees

Des documents plus anciens restent a harmoniser avec l etat actuel :

- `README.md` parle encore d un depot futur et d une revalidation finale de creation du repo dedie
- `KANBAN_OPENCLAW.md` conserve encore `README-OPENCLAW` dans `A OUVRIR`
- `OPENCLAW_REPO_DECISION_REPORT_V1.md` decrit un etat anterieur ou le repo dedie et plusieurs documents de gouvernance n existaient pas encore

Ces contradictions sont documentaires ; elles ne changent pas l etat reel observe.

## 5. Verdict de decision actualise

`repo_dedie_documentaire_valide_runtime_hors_repo`

Ce verdict signifie :
- le repo dedie `openclaw` est legitime comme depot documentaire et de gouvernance
- il ne devient pas pour autant un repo runtime
- le succes de la collecte evidence ne change pas la doctrine de separation
- le runtime, les modules operatoires et les preuves terrain restent portes par `opt-trading` et l infra ciblee

## 6. Point de reprise suivant

Le prochain pas utile n est plus de decider s il faut creer le repo GitHub dedie.

Le prochain pas utile est :

1. harmoniser les documents herites avec ce verdict actualise
2. conserver `OPENCLAW_REPO_DECISION_REPORT_V2.md` comme reference de decision la plus recente
3. ouvrir ensuite `GO_OPENCLAW_CHAIN_03` dans `opt-trading`

## 7. Regles a maintenir

- ne pas melanger repo doc et repo runtime
- ne pas transformer la preuve gateway locale en autorisation de production large
- ne pas relancer KB-301, KB-302 ou KB-303 par simple relecture
- ne pas deplacer du compute standard sur `db-layer`
- ne pas lire `cloud-gpu-future` comme une capacite active

## 8. Condition de mise a jour

Mettre a jour ce rapport seulement si :

- un nouveau verdict de decision repo remplace explicitement ce V2
- la separation documentaire/runtime change formellement
- un nouveau point de reprise officiel est prononce
- une preuve terrain majeure change la lecture de la maturite OpenClaw
