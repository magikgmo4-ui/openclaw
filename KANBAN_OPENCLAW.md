# KANBAN OPENCLAW V1

## 1. But du document

Donner une vue canonique, compacte et operatoire de l etat des chantiers OpenClaw.
Ce kanban sert a lire rapidement ce qui est clos, ce qui reste a ouvrir, ce qui est futur, et ce qui est hors perimetre immediat, sans relire tout le corpus.

## 2. Regle de lecture des statuts

- `CLOSE` : chantier cloture, plus aucune activite prevue dans son perimetre.
- `DONE` : chantier termine dans son perimetre defini ; ne pas le rouvrir par simple relecture.
- `A OUVRIR` : prochain chantier utile non encore traite ou dernier ecart documentaire a fermer.
- `FUTUR` : sujet reconnu mais non actif maintenant.
- `HORS PERIMETRE IMMEDIAT` : sujet explicitement exclu de la passe actuelle.

Regle operative : un chantier clos reste reference canonique tant qu une decision explicite ne cree pas un nouveau chantier distinct.

## 3. CLOSE / DONE

- `PHASE-2-SOCLE` - `CLOSE` - le socle documentaire versionnable KB-201 a KB-205 est fige ; pas de reouverture de doctrine phase 2.
- `KB-201` - `DONE` - registry canonique produit et fige dans le closeout phase 2.
- `KB-202` - `DONE` - validation statique documentaire produite et figee.
- `KB-203` - `DONE` - bootstrap contract documentaire produit et fige.
- `KB-204` - `DONE` - smoke runbook documentaire produit et fige.
- `KB-205` - `DONE` - result contract documentaire produit et fige.
- `KB-301` - `DONE` - `student` est qualifie comme premier compute node local reel ; ne pas le rouvrir par reflexe.
- `KB-302-POLICY` - `DONE` - policy des services exposables sur `student` classee ; ne pas la rouvrir pour un simple besoin de lecture.
- `KB-302-LAB-JUPYTER-CLASSIFICATION` - `DONE` - le cas `lab` + `jupyter` loopback sur `student` est classe documentairement.
- `KB-302-LAB-JUPYTER-LOCAL-OPEN` - `DONE` - le cas `lab` + `jupyter` loopback sur `student` est maintenant prouve en execution reelle borne, sans LAN/public et sans derive de chantier.
- `SESSION-OPENING` - `DONE` - l ouverture minimale de session est maintenant indexee dans `SESSION_OPENING_INDEX.md`.
- `REPO-BOUNDARIES` - `DONE` - le perimetre doc/gouvernance-only du futur repo est borne.
- `WORKFLOW` - `DONE` - les regles de gouvernance, de closeout et de maturite minimale avant repo sont ecrites.
- `ETABLI` - `DONE` - l etat acquis reel, les limites et les non-etablis sont ecrits.

## 4. A OUVRIR

- `README-OPENCLAW` - `A OUVRIR` - dernier ecart documentaire explicite avant revalidation finale du repo dedie ; doit servir de point d entree rapide du futur repo.
- `REVALIDATION-REPO-FINALE` - `A OUVRIR` - a lancer seulement apres presence de `README.md` ; doit rendre le verdict final sur la creation du repo GitHub dedie.

Prochain pas logique immediat : produire `README.md`, puis relancer une revalidation documentaire finale courte.

## 5. FUTUR

- `KB-303` - `FUTUR` - dashboard / UI operateur legere ; sujet reconnu, non actif.
- `KB-304` - `FUTUR` - integration cloud GPU ; cible documentaire future, pas de runtime actif.
- `KB-305` - `FUTUR` - inference / model serving / endpoints GPU ; sujet reconnu, non actif.
- `CLOUD-GPU-FUTURE` - `FUTUR` - target documentaire seulement ; ne pas lire comme capacite disponible.

## 6. HORS PERIMETRE IMMEDIAT

- `CREATION-REPO-GITHUB` - `HORS PERIMETRE IMMEDIAT` - la presente passe ne cree pas le repo ; elle ferme seulement les preconditions documentaires.
- `MIGRATION-GIT` - `HORS PERIMETRE IMMEDIAT` - aucun deplacement, aucune branche, aucune migration de contenu maintenant.
- `RUNTIME-STUDENT-LARGE` - `HORS PERIMETRE IMMEDIAT` - pas d extension runtime large au-dela des preuves deja acquises.
- `SERVICES-PUBLICS-OU-LAN` - `HORS PERIMETRE IMMEDIAT` - aucun service public, aucun tunnel persistant, aucun bind large.
- `DASHBOARD-OPERATEUR` - `HORS PERIMETRE IMMEDIAT` - releve de KB-303, pas de traitement maintenant.
- `INFERENCE-EXPOSEE` - `HORS PERIMETRE IMMEDIAT` - releve de KB-305, pas de traitement maintenant.
- `CLOUD-GPU-RUNTIME` - `HORS PERIMETRE IMMEDIAT` - aucune activation reelle cloud dans cette passe.
- `COMPUTE-STANDARD-SUR-DB-LAYER` - `HORS PERIMETRE IMMEDIAT` - interdit doctrinalement hors exception restreinte deja canonisee.

## 7. Regles anti-reouverture

- Ne pas rouvrir `PHASE-2-SOCLE`, `KB-301` ou `KB-302-POLICY` sans decision explicite ouvrant un nouveau chantier borne.
- Ne pas confondre relecture d un document clos avec reouverture du chantier correspondant.
- Ne pas transformer un manque operatoire sur `student` en fallback vers `db-layer`.
- Ne pas lire un sujet `FUTUR` comme chantier actif.
- Si un besoin nouveau n entre pas clairement dans ce kanban, repartir de `WORKFLOW_OPENCLAW.md` puis `SESSION_OPENING_INDEX.md` avant d ouvrir quoi que ce soit.

## 8. Condition de mise a jour du kanban

Mettre a jour ce kanban seulement si :

- un chantier change formellement de statut ;
- un nouveau chantier canonique est ouvert par decision explicite ;
- un chantier futur devient actif ;
- la revalidation repo change le point de reprise officiel.

Ne pas le mettre a jour comme journal detaille, compte-rendu de session, ou historique narratif.
