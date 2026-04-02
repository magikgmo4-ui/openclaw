# WORKFLOW OPENCLAW V1

## 1. But du workflow

Definir les regles de travail OpenClaw pour qu un nouvel operateur puisse reprendre le chantier sans dependance externe. Le workflow regit comment ouvrir, classifier, valider et cloturer toute activite sur le projet.

## 2. Doctrine OpenClaw

### 2.1 Modele cible

- OpenClaw = control plane Brev-like
- student / cloud-gpu-future = compute plane
- Launchable = unite reproducible de lancement

### 2.2 Roles machines figes

- db-layer = control plane uniquement, pas de compute generaliste
- student = compute local standard
- cloud-gpu-future = cible compute GPU externe future

### 2.3 Doctrine non negotiable

- db-layer ne devient jamais compute plane generaliste
- toute exception sur db-layer doit rester bornee, loopback, non intensive, exceptionnelle
- aucun fallback de convenance vers db-layer si student echoue
- tout compute normal vise student ou cloud-gpu-future

## 3. Sources de verite et ordre de priorite

Quand plusieurs documents existent, l ordre de priorite est:

1. Etatreel valide (runbook terrain, preuve reelle)
2. Spec canonique (SPEC_LAUNCHABLE_OPENCLAW_YAML_V1.md)
3. Runbook (KB-301, KB-302, smoke, bootstrap)
4. Documentation secondaire (exemples, notes)

En cas de contradiction entre documents, la source d indice inferieur ne peut pas contredire une source d indice superieur.

## 4. Regles d ouverture de session

Une session OpenClaw est ouverte quand un operateur definit:

- But: quest-ce que cette session doit accomplishes
- Perimetre: quoi inclure, quoi exclud
- Livrables attendus: fichiers, verifications, decisions
- Point de depart: reference sur socle existant

Une session doitbare un livrable minimal verifiable (pas seulement une liste d intentions).

## 5. Classification des travaux

### 5.1 Cadrage (KB-101 a KB-105, puis KB-301, KB-302)

- Definir la vision, la spec, les exemples, les roles infra
- Produit: documents canoniques de cadrage
- Condition de close: consensus sur la vision et la portee

### 5.2 Validation (KB-202, KB-203, KB-204)

- Valider la forme des launchables sans runtime
- Verifier coherence registry, validation, bootstrap, smoke
- Condition de close: regles de validation passees sans erreur

### 5.3 Micro-chantier (cas unique, borne, delimite)

- Un seul objectif operationnel
- Delai court, resultat observable
- Condition de close: preuve reelleCollectionnee

### 5.4 Implementation bornee

- Extension d un micro-chantier reussi
- Perimetre explicite, pas de derivation
- Condition de close: runbook produit et execute

### 5.5 Closeout (phase 2, puis KB-301 et KB-302)

- Cloturer un chantier en etat stable
- Produit: document de closeout declarant DONE
- Condition de close: plus aucune activite prevue

## 6. Regles de validation / closeout

- Tout chantier doit declarer explicitement son etat: CLOSE, DONE, ou BLOCKED
- Un chantier non-blocked et non-DONE reste en cours
- Un closeout doit nommer le prochain point de reprise
- Aucun chantier ne peut etre declare done sans livrable verificable

## 7. Regles de protection d architecture

### 7.1 Protection de db-layer

- Aucune activation compute standard sur db-layer
- Seule exception: inference locale de validation, bornee, loopback, non intensive, exceptionnelle
- Si un service nest pas authorisable sur student, le verdict reste negatif, pas transporte vers db-layer

### 7.2 Protection de student

- student est le compute local standard par defaut
- Tout compute normal vise student jusqu acloud-gpu-future
- student ne devient pas un deuxieme control plane

### 7.3 Protection de cloud-gpu-future

- Pas d activation runtime reelle sans preconditions externes validees
- Repository entry reste FUTURE tant que preconditions non reunies

## 8. Regles de documentation minimale

Tout livrable doit contenir:

- Chemin exact
- Existence verifiee
- Contenu intelligible
- Pas de placeholders non definis

Tout document de gouvernance doit contenir:

- But du document
- Perimetre / hors-perimetre
- Regles enregistrables
- Condition de mise a jour

## 9. Interdits explicites

- Pas de clone Brev sans decision explicite
- Pas de migration cloud reelle immediate apres formation
- Pas de Jupyter/IDE/API partout des le debut sans cadrage prealable
- Pas de clone UI complet de Brev des le debut
- Pas de chantier NIM/serverless maintenant
- Pas de runtime reel sans preconditions validatees
- Pas de doc marketingfloue ou operationnelle non definie

## 10. Condition de maturite minimale avant repo dedie

Pour qu un repo GitHub dedie OpenClaw soit sainement creable, les conditions minimales sont:

- WORKFLOW_OPENCLAW.md existe et est approuve (CE DOCUMENT)
- REPO_BOUNDARIES_OPENCLAW.md definit les perimetres entre control/compute
- ETABLI_OPENCLAW.md liste machines, tokens, runtimes
- SESSION_OPENING_INDEX.md indexe les sessions
- KANBAN_OPENCLAW.md visualise l etat des chantiers
- README.md pointe l entree du projet

Sans ces documents, le repo ne doit pas etre cree meme si la matiere speculative est presente.