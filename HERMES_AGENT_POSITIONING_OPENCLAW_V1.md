# HERMES AGENT POSITIONING OPENCLAW V1

## 1. But du document

Positionner Hermes Agent dans le cadre OpenClaw sans melanger documentation de gouvernance, runtime effectif et promesse d integration prematuree. Ce document sert de cadrage canonique initial pour toute discussion future sur Hermes dans le perimetre OpenClaw.

## 2. Perimetre

Ce document definit :
- comment Hermes Agent peut etre lu dans le modele OpenClaw ;
- ce qui est autorise a ce stade dans le repo `magikgmo4-ui/openclaw` ;
- ce qui reste explicitement hors perimetre ;
- les prochains livrables documentaires possibles.

Ce document ne definit pas :
- une integration runtime effective de Hermes ;
- une procedure d installation machine ;
- une validation de production ;
- une activation reseau, service ou exposition publique.

## 3. Sources de verite appliquees

Ordre applique pour ce cadrage :

1. etat reel etabli OpenClaw ;
2. doctrine du workflow OpenClaw ;
3. frontieres du repo OpenClaw ;
4. documentation Hermes comme source secondaire a revalider dans ce cadre.

En consequence, Hermes n est pas traite ici comme reference structurante du systeme. Il est traite comme un objet a cadrer dans le modele OpenClaw deja etabli.

## 4. Rappel du modele OpenClaw

Le cadre canonique OpenClaw reste :

- OpenClaw = control plane Brev-like ;
- `db-layer` = control plane uniquement ;
- `student` = compute local standard ;
- `cloud-gpu-future` = cible compute GPU future non active ;
- launchable = unite reproductible de lancement.

Aucune lecture de Hermes ne peut contredire ce cadre.

## 5. Positionnement Hermes dans OpenClaw

### 5.1 Lecture retenue

Dans OpenClaw, Hermes Agent doit etre lu comme un candidat de **pattern agentique documentable** et non comme un runtime a embarquer directement dans le repo.

Hermes peut etre utile comme reference pour :
- memoire persistante d agent ;
- orchestration de taches et sous-agents ;
- structuration de canaux d interaction ;
- execution bornee sur un compute node dedie ;
- separation entre gouvernance de l agent et runtime d execution.

### 5.2 Lecture non retenue

Hermes n est pas retenu ici comme :
- composant natif deja integre a OpenClaw ;
- dependance obligatoire du framework OpenClaw ;
- preuve que le runtime OpenClaw est mature ;
- autorisation implicite de deploiement sur `db-layer`.

## 6. Contraintes doctrinales appliquees a Hermes

Toute etude Hermes dans OpenClaw doit respecter les contraintes suivantes :

- pas de compute generaliste sur `db-layer` ;
- pas de fallback de convenance vers `db-layer` si `student` echoue ;
- pas de secret, token, credential ou config sensible dans le repo ;
- pas de script runtime machine-specifique dans le repo ;
- pas de promesse d exposition reseau large ;
- pas de glissement du repo OpenClaw vers un repo applicatif.

## 7. Ce qui est autorise dans le repo OpenClaw a propos de Hermes

Les livrables suivants sont autorises :

- note de positionnement ;
- spec launchable documentaire ;
- policy de classification ou d exposition ;
- runbook borne de qualification ;
- rapport de verdict ;
- exemple YAML a statut borne si et seulement si son statut est explicite.

## 8. Ce qui reste hors repo a propos de Hermes

Les elements suivants restent hors repo :

- installation Hermes sur machine ;
- scripts shell ou wrappers machine-specifiques ;
- Dockerfiles, images, artefacts et builds ;
- variables d environnement reelles ;
- tokens, cookies, credentials, secrets ;
- logs d execution ;
- etat runtime vivant de Hermes.

## 9. Options de suite documentaire

### Option A - Note de cadrage simple

But : fixer le role Hermes dans OpenClaw sans ouvrir de chantier runtime.

Livrable cible :
- `HERMES_AGENT_POSITIONING_OPENCLAW_V1.md` (present document)
- eventuellement un ajout dans `KANBAN_OPENCLAW.md`

### Option B - Spec launchable Hermes documentaire

But : decrire a quoi ressemblerait un launchable Hermes borne, sans l activer.

Livrable cible :
- `SPEC_LAUNCHABLE_HERMES_AGENT_OPENCLAW_V1.md`
- exemple YAML associe a statut explicite

### Option C - Qualification bornee sur student

But : ouvrir plus tard un micro-chantier borne pour verifier si un cas Hermes local est classable sur `student`.

Precondition :
- decision explicite de sortir du simple cadrage documentaire ;
- runbook et policy de securite alignes ;
- aucun glissement vers `db-layer`.

## 10. Verdict actuel

Le verdict actuel est le suivant :

- Hermes Agent est admissible comme **objet de cadrage documentaire** dans OpenClaw ;
- Hermes Agent n est pas encore etabli comme runtime OpenClaw ;
- aucun deploiement ou packaging runtime ne doit etre infere depuis ce document ;
- toute suite doit rester gouvernee par les frontieres documentaires deja etablies.

## 11. Condition de mise a jour

Mettre a jour ce document si et seulement si :

- une decision canonique ouvre une spec Hermes plus detaillee ;
- un launchable Hermes documentaire devient necessaire ;
- un micro-chantier borne de qualification Hermes est explicitement ouvert ;
- les frontieres OpenClaw changent.

Ne pas mettre a jour ce document pour ajouter :
- notes de session brutes ;
- logs runtime ;
- secrets ;
- commandes machine temporaires ;
- speculation non classee.

## 12. Point de reprise

Point de reprise canonique apres ce document :

- soit ouvrir une spec Hermes documentaire ;
- soit classer Hermes comme simple reference sans suite immediate ;
- soit preparer un micro-chantier borne sur `student` si et seulement si un GO explicite l autorise.
