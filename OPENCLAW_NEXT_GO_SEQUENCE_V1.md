# OPENCLAW NEXT GO SEQUENCE V1

## 1. But du document

Figer le plan de reprise OpenClaw sous forme de sequence courte, ordonnee et verificable, en distinguant clairement :

- ce qui releve de `opt-trading` pour l etat reel et les modules operatoires ;
- ce qui releve du repo `openclaw` pour la gouvernance et la revalidation documentaire.

Ce document ne cree ni nouveau runtime, ni nouveau perimetre. Il sert de point de reprise compact pour enchainer les prochains GO sans re-ouvrir les chantiers clos.

## 2. Regles de lecture

- `opt-trading` reste la source operative pour les modules, les preuves et l etat reel.
- `openclaw` reste doc/gouvernance-only.
- aucun GO ci-dessous n autorise de compute standard sur `db-layer`.
- aucun GO ci-dessous n autorise une exposition reseau large, un runtime cloud actif, ou un serving GPU expose.
- les chantiers `KB-301`, `KB-302` et `KB-303` restent des references closes ; ils ne sont pas reouverts par simple relecture.

## 3. Sequence canonique des prochains GO

### GO_OPENCLAW_EVIDENCE_01
**Type** : diagnostic ponctuel

**Repo principal** : `opt-trading`

**But** :
capturer un etat reel, propre et exploitable avant toute revalidation documentaire.

**Base operative** :
- `doctor_openclaw`
- `configure_openclaw`
- `evidence_openclaw`

**Sortie attendue** :
un pack de preuves courant permettant de documenter :
- statut doctor ;
- statut configure ;
- quick checks ;
- contexte workspace ;
- prompt documentaire derive des preuves.

**Condition de close** :
les preuves existent, sont lisibles, et permettent une relecture factuelle sans inference libre.

### GO_OPENCLAW_SYNC_02
**Type** : patch local documentaire

**Repo principal** : `openclaw`

**But** :
re-aligner les documents de gouvernance avec l etat reel du repo et avec les preuves exportees depuis `opt-trading`.

**Perimetre minimal** :
- verifier `README.md`
- verifier `KANBAN_OPENCLAW.md`
- verifier `OPENCLAW_REPO_DECISION_REPORT_V1.md`
- corriger les ecarts de point de reprise si necessaire

**Condition de close** :
le repo `openclaw` ne porte plus de contradiction interne sur :
- le statut du `README.md`
- le prochain point de reprise
- le statut de la revalidation finale

### GO_OPENCLAW_CHAIN_03
**Type** : module durable

**Repo principal** : `opt-trading`

**But** :
consolider la chaine operateur standard OpenClaw en parcours reproductible et borne.

**Chaine cible** :
`install_module_openclaw` -> `openclaw_config_modulaire` -> `gateway_openclaw` -> `configure_openclaw` -> `doctor_openclaw` -> `evidence_openclaw`

**Condition de close** :
un operateur peut suivre un parcours unique, lisible et standard pour :
- installer ;
- configurer ;
- lancer ;
- verifier ;
- collecter les preuves ;
sans sequence ad hoc.

### GO_OPENCLAW_PROVIDER_POLICY_04
**Type** : module durable

**Repo principal** : `opt-trading`

**But** :
brancher effectivement `model_provider_openclaw` dans la chaine OpenClaw deja standardisee.

**Perimetre minimal** :
- installation via le flux standard ;
- validation / sanity coherente ;
- usage borne avec `configure_openclaw` et `doctor_openclaw` si necessaire ;
- preuves exportables dans la chaine evidence.

**Condition de close** :
la policy provider/model n est plus seulement doctrinale ; elle devient operable, validable et relisible.

### GO_HERMES_OPENCLAW_BRIDGE_05
**Type** : patch local borne

**Repo principal** : `opt-trading`

**But** :
produire une premiere preuve courte du bridge Hermes -> OpenClaw -> validation humaine.

**Garde-fous** :
- pas d auto-commit ;
- pas d execution non controlee ;
- validation humaine obligatoire avant integration.

**Condition de close** :
une preuve de bout en bout existe sur un cas borne, sans derive produit ni confusion repo/runtime.

## 4. Ordre recommande

1. `GO_OPENCLAW_EVIDENCE_01`
2. `GO_OPENCLAW_SYNC_02`
3. `GO_OPENCLAW_CHAIN_03`
4. `GO_OPENCLAW_PROVIDER_POLICY_04`
5. `GO_HERMES_OPENCLAW_BRIDGE_05`

Regle operative :
ne pas ouvrir `GO_OPENCLAW_CHAIN_03` ni `GO_OPENCLAW_PROVIDER_POLICY_04` tant que `GO_OPENCLAW_EVIDENCE_01` n a pas produit un etat reel relisible, et que `GO_OPENCLAW_SYNC_02` n a pas fige le point de reprise documentaire.

## 5. Ce qui n entre pas dans cette sequence

- `KB-304` cloud GPU runtime
- `KB-305` inference / serving GPU expose
- services LAN/publics larges
- extension runtime large sur `student`
- compute standard sur `db-layer`
- creation d un nouveau repo runtime

Ces sujets restent futurs ou hors perimetre immediat.

## 6. Point de reprise officiel

Le point de reprise officiel apres ce document est :

1. sortir les preuves reelles via `opt-trading`
2. resynchroniser le repo `openclaw`
3. seulement ensuite ouvrir les modules durables suivants

Aucun saut direct vers cloud, serving expose ou runtime large n est autorise par ce document.

## 7. Condition de mise a jour

Mettre a jour ce document seulement si :

- l ordre des GO change formellement ;
- un GO est cloture et remplace par un nouveau point de reprise ;
- un element actuellement hors perimetre devient officiellement actif ;
- la separation `opt-trading` / `openclaw` change par decision explicite.

Ne pas l utiliser comme journal de session, backlog exhaustif, ou compte-rendu narratif.
