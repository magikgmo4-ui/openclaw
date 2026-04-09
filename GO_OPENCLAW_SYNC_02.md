# GO_OPENCLAW_SYNC_02

## 1. Classification

Type : patch local documentaire

## 2. But

Resynchroniser le repo `openclaw` avec l etat reel le plus recent prouve depuis `opt-trading`, sans melanger repo documentaire et repo runtime.

Ce GO ne cree aucun nouveau runtime. Il borne seulement le point de reprise documentaire apres execution reelle de `GO_OPENCLAW_EVIDENCE_01`.

## 3. Sources de verite pour ce GO

### 3.1 Source terrain

Execution reelle de `GO_OPENCLAW_EVIDENCE_01` sur `db-layer`, sous l utilisateur `openclaw`, avec preuves exportees dans :

`/home/openclaw/.openclaw/workspace-orchestrateur/docs_evidence/openclaw_current_state/`

Fichiers rafraichis :
- `01_doctor_status.txt`
- `02_configure_status.txt`
- `03_doctor_quick.txt`
- `04_workspace_context.txt`
- `90_PROMPT_DOCS.txt`

### 3.2 Faits reels etablis durant la collecte

- le workspace detecte est `workspace-orchestrateur`
- le premier essai a rencontre un gateway loopback ferme (`1006 abnormal closure`)
- `gateway_openclaw` a confirme `SESSION_STATUS=stopped`
- le gateway a ensuite ete demarre
- `gateway health` est revenu `OK`
- `gateway probe` est revenu `Reachable: yes`, `Connect: ok`, `RPC: ok`
- `gateway_openclaw` a ensuite confirme `SESSION_STATUS=running`
- l export evidence a alors pu regenerer les 5 fichiers attendus

## 4. Contradictions documentaires a corriger

Les documents suivants ne sont plus parfaitement alignes avec l etat reel actuel du repo :

- `README.md` parle encore d un depot futur et d une revalidation finale de creation du repo dedie
- `KANBAN_OPENCLAW.md` parle encore de `README-OPENCLAW` a ouvrir et d une revalidation immediate apres presence du README
- `OPENCLAW_REPO_DECISION_REPORT_V1.md` parle encore d un etat ou le repo dedie et plusieurs documents de gouvernance n existaient pas

## 5. Decision documentaire de cette passe

Pour cette passe, le point de reprise canonique devient :

1. `GO_OPENCLAW_EVIDENCE_01` = execute et clos cote collecte de preuves
2. le repo `openclaw` existe deja comme repo doc/gouvernance-only
3. la prochaine action utile n est plus de decider s il faut creer le repo
4. la prochaine action utile est d harmoniser les documents herites avec l etat reel actuel

## 6. Point de reprise officiel apres sync

Une fois ce GO relu :

- utiliser `OPENCLAW_REPO_DECISION_REPORT_V2.md` comme reference de decision la plus recente
- ne pas rouvrir les KB clos
- ne pas inferer une maturite runtime large a partir du succes du gateway loopback
- preparer ensuite `GO_OPENCLAW_CHAIN_03` dans `opt-trading`

## 7. Hors perimetre

- aucune activation cloud GPU
- aucun serving modele expose
- aucun compute standard sur `db-layer`
- aucune migration de runtime dans le repo `openclaw`
- aucune confusion entre preuves terrain et autorisation de production large

## 8. Condition de close

Ce GO est considere relisible si :

- la preuve d execution de `GO_OPENCLAW_EVIDENCE_01` est figee
- le point de reprise ne parle plus d un repo encore a creer
- la decision repo la plus recente est documentee dans un rapport explicite
