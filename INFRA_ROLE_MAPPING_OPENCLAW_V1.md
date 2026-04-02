# INFRA ROLE MAPPING OPENCLAW V1

## 1. Objet

Ce document fige la repartition des roles entre les machines de l'infrastructure OpenClaw Brev-like V1.

## 2. Roles canoniques

| Machine / role | Statut V1 | Fonction principale |
|---|---|---|
| `db-layer` | control plane | heberger OpenClaw, les definitions, l'administration et la gouvernance |
| `student` | compute lab local | executer les launchables locaux et servir de premier node Brev-like reel |
| `cloud-gpu-future` | compute GPU externe | executer plus tard les launchables GPU externes avec isolation et capacite superieures |

## 3. Responsibilities par machine

### `db-layer`
- heberge le control plane OpenClaw ;
- porte les definitions versionnees, les policies, les presets et la logique d'administration ;
- sert de point de reference pour la validation locale et les usages operatoires limites ;
- peut tolerer un launchable `inference` local de validation si l'usage reste borne, loopback et non intensif.

### `student`
- porte le compute local principal de la V1 ;
- execute les profils `lab`, `vision`, `hf_build` et, si necessaire, `training` leger ;
- sert de premiere cible de mise en pratique avant toute extension cloud ;
- ne doit pas devenir le point de verite des policies ou de la spec.

### `cloud-gpu-future`
- represente la cible de compute GPU externe a preparer, sans activation immediate ;
- recevra a terme les profils `training` lourds et `inference` GPU ;
- reste hors implementation runtime dans cette passe ;
- doit etre traite comme trust boundary distinct du local.

## 4. Regles de non-melange

- Le control plane ne doit pas etre deplace sur `student`.
- `db-layer` ne doit pas devenir une machine de compute generaliste.
- `student` ne doit pas heberger les secrets, politiques et fonctions de gouvernance comme source unique.
- `cloud-gpu-future` ne doit pas etre suppose disponible tant que son chantier dedie n'est pas ouvert.
- Un launchable doit cibler un seul `node.target` et ne pas melanger plusieurs roles dans le meme fichier.

## 5. Lecture operationnelle

### Ce qui est autorise en V1
- gerer depuis `db-layer` ;
- executer localement sur `student` ;
- preparer declarativement `cloud-gpu-future` sans le brancher reellement ;
- utiliser `db-layer` seulement pour de l'inference locale de validation strictement encadree.

### Ce qui n'est pas autorise en V1
- transformer `db-layer` en node GPU polyvalent ;
- exposer par defaut des services publics depuis n'importe quelle machine ;
- confondre le stockage documentaire canonique avec un workspace d'execution volatile ;
- traiter `student` comme control plane de secours permanent.

## 6. Mapping launchables -> cibles recommandees

| Profil | Cible recommandee V1 | Remarque |
|---|---|---|
| `lab` | `student` | usage interactif local |
| `training` | `student` ou `cloud-gpu-future` | `cloud-gpu-future` reste preparatoire |
| `inference` | `db-layer` ou `cloud-gpu-future` | `db-layer` uniquement pour validation locale bornee |
| `vision` | `student` | usage local de dev / test |
| `hf_build` | `student` | construction et preparation locale |

## 7. Decision de gouvernance

La V1 retient un modele simple :
- `db-layer` gouverne ;
- `student` execute localement ;
- `cloud-gpu-future` execute plus tard a distance.

Toute extension future devra conserver cette separation, sauf decision explicite de revision d'architecture.
