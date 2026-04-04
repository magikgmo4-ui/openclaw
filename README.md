# OpenClaw

OpenClaw est un control plane Brev-like documente. Ce depot futur sert a versionner la gouvernance, les specs, les garde-fous et les exemples OpenClaw ; il ne sert pas a embarquer le runtime effectif.

## Perimetre du repo

Le repo OpenClaw est un repo doc/gouvernance-only.

Il documente :

- le modele OpenClaw ;
- les launchables canoniques ;
- les roles machines ;
- les policies de securite et d exposition ;
- les runbooks et rapports de qualification ;
- l etat des chantiers et les points d entree operatoires.

Il ne remplace ni `opt-trading`, ni une infra runtime, ni un workspace d execution.

## Ce que contient le repo

- gouvernance : `WORKFLOW_OPENCLAW.md`, `REPO_BOUNDARIES_OPENCLAW.md`, `ETABLI_OPENCLAW.md`, `SESSION_OPENING_INDEX.md`, `KANBAN_OPENCLAW.md`
- cadrage : `OPENCLAW_BREV_LIKE_MASTER_PLAN_V1.md`, `INFRA_ROLE_MAPPING_OPENCLAW_V1.md`, `SECURITY_POLICY_OPENCLAW_BREV_V1.md`
- spec et validation : `SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md`, `launchables_registry.yaml`, `VALIDATION_RULES_LAUNCHABLES_V1.md`, `VALIDATION_MATRIX_LAUNCHABLES_V1.md`, `bootstrap_contract_v1.md`, `RUNBOOK_LAUNCHABLE_SMOKE_V1.md`, `RESULT_CONTRACT_V1.md`
- runbooks et rapports : `PHASE2_CLOSEOUT_CANONICAL_V1.md`, `KB301_STUDENT_NODE_INTEGRATION_RUNBOOK_V1.md`, `KB301_STUDENT_REAL_QUALIFICATION_REPORT_V1.md`, `KB302_EXPOSABLE_SERVICES_POLICY_V1.md`, `KB302_LAB_JUPYTER_CLASSIFICATION_REPORT_V1.md`, `KB302_LAB_JUPYTER_LOOPBACK_RUNBOOK_V1.md`
- maquette locale : `KB303_MINIMAL_MOCKUP_V1.html`
- exemples : `examples/launchable_lab_student.yaml`, `examples/launchable_vision_student.yaml`, `examples/launchable_hf_build_student.yaml`, `examples/launchable_training_cloud_future.yaml`, `examples/launchable_inference_db_layer.yaml`

## Ce que le repo ne contient pas

- code executable de production ;
- scripts d installation ou de demarrage machine-specifiques ;
- Dockerfiles, builds, artefacts ou logs runtime ;
- secrets, tokens, credentials ou configurations sensibles ;
- migration depuis `opt-trading` ;
- promesse de cloud GPU actif ou de services exposes par defaut.

## Etat actuel du projet

- OpenClaw reste doc/gouvernance-first.
- `db-layer` reste control plane uniquement ; pas de compute generaliste.
- `student` est le compute local standard deja qualifie.
- `cloud-gpu-future` reste une cible documentaire future, non active.
- le socle documentaire phase 2 est `CLOSE`.
- KB-301 est `DONE`.
- KB-302 policy est `DONE`.
- la maturite runtime reste volontairement limitee et ne doit pas etre surevaluee.

## Demarrage rapide documentaire

Pour une premiere lecture ou une reprise propre :

1. `WORKFLOW_OPENCLAW.md`
2. `REPO_BOUNDARIES_OPENCLAW.md`
3. `ETABLI_OPENCLAW.md`
4. `SESSION_OPENING_INDEX.md`
5. `KANBAN_OPENCLAW.md`

Si besoin ensuite :

- vision et cadrage : `OPENCLAW_BREV_LIKE_MASTER_PLAN_V1.md`
- spec et registry : `SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md`, `launchables_registry.yaml`
- point de cloture du socle : `PHASE2_CLOSEOUT_CANONICAL_V1.md`

## Documents cles

- `WORKFLOW_OPENCLAW.md` : regles de travail, closeout, maturite minimale avant repo dedie
- `REPO_BOUNDARIES_OPENCLAW.md` : perimetre exact du futur repo et frontieres a ne pas melanger
- `ETABLI_OPENCLAW.md` : etat acquis reel, non-etablis et limites
- `SESSION_OPENING_INDEX.md` : ordre de lecture et parcours par type de session
- `KANBAN_OPENCLAW.md` : etat compact des chantiers clos, a ouvrir et futurs

## Point de reprise actuel

Le point de reprise documentaire actuel est simple :

- le socle canonique est en place ;
- le dernier ecart documentaire majeur est ferme par ce `README.md` ;
- le prochain pas attendu est une revalidation finale de creation du repo GitHub dedie ;
- aucun repo ne doit etre cree par reflexe avant ce verdict final.

## Limites explicites

- Ne pas lire ce repo comme une plateforme runtime prete a deploiement.
- Ne pas lire `cloud-gpu-future` comme une capacite disponible.
- Ne pas deplacer du compute standard sur `db-layer`.
- Ne pas inferer une UI operateur, un serving modele, ou une exposition reseau large a partir de la seule documentation.
- En cas de doute, repartir de `WORKFLOW_OPENCLAW.md`, `REPO_BOUNDARIES_OPENCLAW.md` et `ETABLI_OPENCLAW.md`.
