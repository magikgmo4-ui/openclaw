# OPENCLAW BREV-LIKE MASTER PLAN V1

## 1. But

Figer une base canonique V1 pour utiliser OpenClaw comme control plane dans une logique Brev-like, sans lancer une refonte runtime ni chercher un clone fonctionnel complet de NVIDIA Brev.

Le livrable de cette passe est documentaire, versionnable et directement exploitable pour la suite du chantier.

## 2. Vision cible

### Modèle d'ensemble
- OpenClaw porte le control plane : description des launchables, ciblage des nodes, bootstrap, policy, services et garde-fous.
- Les machines d'execution portent le compute plane : `student` pour le lab local, `cloud-gpu-future` pour le GPU externe quand il existera.
- Un Launchable OpenClaw est l'unite declarative reproductible qui exprime un usage donne : `lab`, `training`, `inference`, `vision`, `hf_build`.

### Resultat vise en V1
La V1 doit permettre de lire et versionner, sans ambiguite :
- quel agent doit etre lance ;
- dans quel workspace ;
- sur quel node ;
- avec quel bootstrap ;
- sous quelle policy ;
- avec quels services eventuels.

## 3. Perimetre

Cette passe couvre uniquement :
- le cadre directeur OpenClaw Brev-like ;
- la spec canonique du YAML Launchable ;
- le mapping des roles d'infrastructure ;
- la doctrine securite minimale ;
- des exemples YAML credibles et reutilisables.

## 4. Hors perimetre

Cette passe ne couvre pas :
- l'implementation runtime reelle a grande echelle ;
- l'integration cloud GPU effective ;
- un clone UI de Brev ;
- une ouverture generalisee de Jupyter, IDE, API ou tunnels ;
- le chantier NIM, serverless ou model serving avance ;
- le registry, la validation statique, le bootstrap contract detaille et les runbooks smoke, qui appartiennent a la phase suivante.

## 5. Mapping Brev-like -> OpenClaw

| Logique Brev-like | Equivalent OpenClaw V1 | Decision V1 |
|---|---|---|
| Workspace launchable | `Launchable` YAML versionne | unite canonique de lancement |
| Control plane | OpenClaw sur `db-layer` | point de pilotage et de gouvernance |
| Compute local lab | node `student` | premier terrain d'execution concret |
| Compute GPU externe | node `cloud-gpu-future` | cible reservee pour extension ulterieure |
| Profil d'usage | `spec.profile` | valeurs bornees : `lab`, `training`, `inference`, `vision`, `hf_build` |
| Bootstrap du job | `spec.bootstrap` | contrat minimal, pas encore implementation detaillee |
| Garde-fous reseau/auth | `spec.policy` | deny by default, loopback par defaut |
| Services attaches au lancement | `spec.services` | explicites, rares, et jamais implicites |

## 6. Roles cibles

- `db-layer` : hote de control plane, d'orchestration et d'administration. Son usage compute doit rester borne aux besoins de validation locale lies au control plane.
- `student` : node de compute local pour les usages `lab`, `vision`, `hf_build` et, si necessaire, `training` leger.
- `cloud-gpu-future` : node de compute GPU externe pour `training` et `inference` a capacite et isolation superieures, apres ouverture du chantier dedie.

## 7. Phases d'execution

### Phase 1 - Cadrage documentaire obligatoire
1. Ecrire le master plan.
2. Figer la spec Launchable V1.
3. Produire les exemples YAML.
4. Figer le mapping infra.
5. Figer la doctrine securite.

### Phase 2 - Socle versionnable
1. Introduire un registry canonique des launchables.
2. Ajouter une validation statique du format.
3. Ecrire le bootstrap contract minimal.
4. Ajouter un runbook de smoke.
5. Definir le contrat de logs et d'artefacts.

### Phase 3 - Premiere activation reelle
1. Integrer `student` comme premier compute node Brev-like reel.
2. Ouvrir la politique des services exposables avec garde-fous.
3. Ajouter une lecture operateur legere.

### Phase 4 - Extension
1. Integrer `cloud-gpu-future`.
2. Ouvrir separement le chantier inference / serving.

## 8. Principaux risques

- Confondre Brev-like et clone de Brev, ce qui ferait deriver le perimetre.
- Faire trop d'implementation avant d'avoir une spec stable.
- Melanger control plane et compute plane, en particulier sur `db-layer`.
- Laisser des services implicites ou des ouvertures reseau non gouvernees.
- Produire des launchables trop abstraits pour etre validables.

## 9. Doctrine d'implementation

- Document-first : la spec precede le runtime.
- Declaratif avant implicite : tout launchable doit expliciter agent, workspace, node, bootstrap, policy et services.
- Valeurs bornees : les profils, roles de nodes et modes de policy sont controles.
- Default deny : aucun service n'est expose par defaut.
- Separation claire : `db-layer` gouverne, `student` execute localement, `cloud-gpu-future` execute a distance quand il sera pret.
- Extension par phases : chaque brique future doit prolonger la V1 sans casser son contrat.

## 10. Critere de sortie de la V1 documentaire

La V1 est exploitable si un operateur peut relire le pack et savoir, sans recadrage supplementaire :
- comment decrire un launchable ;
- quelles cibles d'infrastructure sont autorisees ;
- quelles limites de securite s'appliquent ;
- quels exemples servent de base de reference.
