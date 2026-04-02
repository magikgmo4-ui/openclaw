# KB301 STUDENT NODE INTEGRATION RUNBOOK V1

## 1. But de KB-301

KB-301 existe pour ouvrir une seule activation reelle, mais bornee :
qualifier `student` comme premier node compute local reel d'OpenClaw.

La question canonique est unique :

> `student` peut-il etre declare premier compute node local reel, sans ambiguite d'architecture, sans requalifier `db-layer`, et sans ouvrir KB-302 a KB-305 trop tot ?

KB-301 ne cherche pas a faire un clone Brev. KB-301 ne cherche pas a ouvrir tous les profils, tous les services, ni tout le runtime. KB-301 cherche a declarer proprement un premier node compute local standard.

## 2. Rappel du socle canonique

- Le closeout phase 2 est `CLOSE` et reste fige.
- Le socle canonique reutilisable tel quel est :
  - `PHASE2_CLOSEOUT_CANONICAL_V1.md`
  - `launchables_registry.yaml`
  - `VALIDATION_RULES_LAUNCHABLES_V1.md`
  - `VALIDATION_MATRIX_LAUNCHABLES_V1.md`
  - `bootstrap_contract_v1.md`
  - `RUNBOOK_LAUNCHABLE_SMOKE_V1.md`
  - `RESULT_CONTRACT_V1.md`
- KB-301 ne reouvre pas KB-201 a KB-205.
- KB-301 reutilise les compatibilites `profile -> node`, les garde-fous reseau/auth, les classes bootstrap/smoke et le canon de resultat deja figes.
- Aucun ajustement local ne doit contredire le registry, la validation statique, le bootstrap contract, le smoke runbook ou le result contract.

## 3. Verrou doctrinal obligatoire

- `student` = premier compute node local reel.
- `db-layer` = control plane OpenClaw.
- `db-layer` n'est pas un compute plane generaliste.
- Seule exception admise sur `db-layer` : `inference` locale de validation, bornee, loopback, non intensive, exceptionnelle.
- Tout compute normal vise `student` ou, plus tard, `cloud-gpu-future`.

Regle operative non negociable :

- aucun "petit glissement pratique" n'autorise du compute standard sur `db-layer` ;
- aucun fallback de convenance vers `db-layer` n'est admis si `student` echoue ;
- si `student` ne passe pas, le verdict est `non_integre`, pas "on lance sur db-layer en attendant".

## 4. Profils Launchable autorises sur `student`

### 4.1 Profils naturels sur `student`

- `lab`
- `vision`
- `hf_build`
- `training` leger local, seulement si un launchable `student` existe au registry plus tard

### 4.2 Profils admis maintenant dans KB-301

Les profils admis comme base de qualification KB-301 sont :

- `hf_build` sur `student` : base de preuve recommandee, car compute borne sans sujet principal de service expose ;
- `lab` sur `student` : admissible si on reste strictement en boucle locale et sans ouvrir KB-302 ;
- `vision` sur `student` : admissible seulement si le prerequis GPU local est reellement tenu et prouve.

Lecture pratique :

- `examples/launchable_hf_build_student.yaml` = preuve standard minimale recommandee ;
- `examples/launchable_lab_student.yaml` = preuve locale interactive admissible, sans en faire un chantier services ;
- `examples/launchable_vision_student.yaml` = extension optionnelle si la machine revendique une capacite GPU locale.

### 4.3 Profils raisonnables mais non a absorber maintenant

- `training` sur `student` reste architecturalement raisonnable selon la doctrine, mais n'est pas une preuve minimale KB-301 tant qu'un launchable `training` sur `student` n'est pas explicitement enregistre et borne.
- `inference` n'est pas un profil `student` dans le canon V1 actuel. Il reste soit exception restreinte sur `db-layer`, soit cible future `cloud-gpu-future`.

## 5. Prerequis minimaux d'integration

Les prerequis minimaux ci-dessous sont obligatoires avant toute declaration `integre`.

### P-01 Accessibilite machine

- `student` est joignable par le chemin operatoire normal ;
- une session locale ou distante exploitable est possible ;
- l'execution ne depend pas de detourner `db-layer` pour faire le compute.

### P-02 Identite node claire

- l'identite de la machine qualifiee comme `student` est stable et non ambigue ;
- `student` est lisible comme `compute-local` et non comme control plane bis ;
- aucun doute n'existe entre `student`, `db-layer` et `cloud-gpu-future`.

### P-03 Environnement minimal coherent

- un contexte d'execution local existe pour les launchables retenus ;
- le mode de sandbox requis par le profil choisi est soutenable sur `student` ;
- rien de bloquant n'interdit par construction tous les launchables `student` admis.

### P-04 Capacite de workspace locale

- le `workspace.root` du launchable retenu peut exister sur `student` ;
- le chemin est creatible ou deja present ;
- le chemin est writable ;
- la persistence locale promise par le launchable est tenable.

### P-05 Capacite bootstrap minimale

- le preset de bootstrap et l'env non secretes du launchable retenu peuvent etre prepares sur `student` ;
- cette preparation reste locale, bornee et sans dependance cloud structurante ;
- aucune ouverture publique, aucun tunnel, aucune UI operateur n'est necessaire pour passer.

### P-06 Capacite d'execution bornee

- au moins un launchable `student` standard peut executer un passage borne sur la machine ;
- ce passage demarre, produit des traces minimales, puis s'arrete proprement ;
- aucune execution longue, orchestration complete ou service expose durable n'est requise.

### P-07 Capacite de logs / traces minimales

- le launchable retenu peut produire au minimum un exit code, une trace stdout/stderr ou un log local, et un chemin d'artefacts ou de logs ;
- ces traces sont recuperables comme preuves simples ;
- l'absence d'observabilite complete n'est pas bloquante, mais l'absence totale de traces l'est.

### P-08 Coherence canonique

- le launchable retenu existe dans `launchables_registry.yaml` ;
- il est compatible avec KB-202 ;
- il reste `phase-2-early` autorise ;
- sa lecture bootstrap/smoke reste `standard` sur `student` ;
- les sorties minimales peuvent etre exprimees selon `RESULT_CONTRACT_V1.md`.

### P-09 Garde-fou doctrinal

- la qualification de `student` ne requiert ni KB-302, ni KB-304, ni KB-305 ;
- la qualification de `student` ne requiert pas de convertir `db-layer` en secours compute ;
- la qualification de `student` ne requiert pas de reouvrir le closeout phase 2.

### Prerequis conditionnels par profil

- revendiquer `vision` sur `student` exige une capacite GPU locale reelle et observable ;
- revendiquer `hf_build` avec telechargements reels peut exiger un egress controle, jamais une exposition entrante ;
- revendiquer `lab` n'exige pas d'ouverture publique : loopback strict seulement si un bind est effectivement verifie.

## 6. Sequence de qualification

La sequence de qualification KB-301 est volontairement simple et bornee.

### Etape 1 - Verrouiller la lecture canonique

- relire le closeout phase 2 ;
- relire le registry et les regles de validation ;
- fixer que `student` est le compute local standard et que `db-layer` ne change pas de role.

### Etape 2 - Choisir le perimetre de preuve

- choisir un launchable `student` de base ;
- choix recommande : `hf-build-student` ;
- choix admissible alternatif : `lab-student` ;
- ajouter `vision-student` seulement si une revendication GPU locale fait partie de la qualification.

### Etape 3 - Controler les prerequis machine

- verifier accessibilite ;
- verifier identite du node ;
- verifier workspace local ;
- verifier repertoire de logs et d'artefacts ;
- verifier que le mode de sandbox minimal du launchable retenu est soutenable.

### Etape 4 - Controler la coherence documentaire

- verifier la presence du launchable au registry ;
- verifier la compatibilite `profile -> student` ;
- verifier la validite statique KB-202 ;
- verifier la lecture bootstrap KB-203 ;
- verifier la lecture smoke KB-204 ;
- verifier la compatibilite des sorties avec KB-205.

### Etape 5 - Executer une qualification bornee sur `student`

- preparer le workspace local ;
- preparer le bootstrap minimal du launchable retenu ;
- executer un passage borne sur `student` ;
- produire une trace minimale observable ;
- arreter proprement ;
- ne pas transformer la verification en service durable, en tunneling ou en mini orchestrateur.

### Etape 6 - Collecter les preuves minimales

- relever l'identite du node ;
- relever le launchable qualifie ;
- relever les chemins workspace / logs / artefacts ;
- relever le resultat documentaire compatible KB-205 ;
- relever la preuve d'execution bornee ;
- relever explicitement l'absence de derive vers `db-layer` ou KB-302+.

### Etape 7 - Emettre le verdict

- `integre` si tous les prerequis obligatoires et toutes les preuves minimales sont presents ;
- `non_integre` des qu'un blocker apparait ou qu'un prerequis obligatoire manque.

Regle d'arret immediate : des qu'un blocker est rencontre, on stoppe la qualification. On n'elargit pas le chantier pour forcer un succes.

## 7. Preuves minimales attendues

Le minimum exigible pour declarer `student` integre est le suivant.

### E-01 Preuve d'identite node

- identifiant exploitable du node qualifie comme `student` ;
- confirmation que ce node porte la fonction `compute-local`.

### E-02 Preuve d'accessibilite

- acces operatoire reussi au node ;
- contexte d'execution local disponible.

### E-03 Preuve de workspace

- `workspace.root` du launchable retenu creatible ou present ;
- ecriture locale possible ;
- persistence locale compatible avec le launchable.

### E-04 Preuve logs / artefacts

- chemin `artifactsPath` ou equivalent ;
- chemin `logsPath` ou equivalent ;
- au moins une trace observable locale.

### E-05 Preuve de coherence documentaire

- match Launchable <-> Registry ;
- lecture KB-202 compatible ;
- lecture KB-203 compatible ;
- lecture KB-204 compatible ;
- noyau de resultat exprimable selon KB-205.

### E-06 Preuve d'execution bornee

- un launchable `student` standard a execute un passage borne sur `student` ;
- le passage a produit un statut observable ;
- le passage s'est termine proprement.

### E-07 Preuve de non-derive doctrinale

- aucun compute standard n'a ete reporte sur `db-layer` ;
- aucune ouverture publique, aucun tunnel public, aucun chantier cloud GPU n'a ete requis ;
- la qualification ne depend pas d'une UI operateur ni d'un orchestrateur complet.

### E-08 Preuve conditionnelle GPU

- obligatoire seulement si `vision` est revendique sur `student` ;
- capacite GPU locale observable et utilisable sur un passage borne.

Sans ce paquet minimal de preuves, `student` reste `non_integre`.

## 8. Blockers et conditions d'arret

Les points suivants bloquent KB-301 et conduisent a `non_integre`.

- B-01 : le node qualifie comme `student` n'est pas identifiable clairement ;
- B-02 : la machine n'est pas joignable ou n'offre pas de contexte d'execution exploitable ;
- B-03 : le workspace local n'est pas tenable ou writable ;
- B-04 : les chemins minimaux de logs / artefacts ne sont pas tenables ;
- B-05 : le launchable choisi est incoherent avec le registry, la validation, le bootstrap ou le smoke canonique ;
- B-06 : aucun launchable `student` standard ne peut soutenir un passage borne ;
- B-07 : la qualification exige un contournement via `db-layer` ;
- B-08 : la qualification exige d'ouvrir KB-302, KB-304 ou KB-305 pour reussir ;
- B-09 : la qualification exige une exposition publique, un tunnel persistant, une UI operateur ou un orchestrateur complet ;
- B-10 : aucune preuve minimale observable ne peut etre collecte ;
- B-11 : une revendication `vision` est faite sans prerequis GPU reel ;
- B-12 : un `training` sur `student` est tente comme preuve canonique alors qu'aucun launchable `student` correspondant n'est enregistre et borne.

## 9. Verdicts autorises

### `integre`

`student` peut etre declare `integre` si et seulement si :

- tous les prerequis obligatoires sont vrais ;
- au moins un launchable `student` standard a une preuve reelle d'execution bornee ;
- les preuves minimales sont collectees ;
- aucune derive vers `db-layer` ou KB-302+ n'a ete necessaire.

Important : `integre` signifie "premier node compute local reel qualifie". Cela ne signifie pas que tous les profils optionnels imaginables sont automatiquement valides sur `student`.

### `non_integre`

`student` est `non_integre` si :

- un prerequis obligatoire manque ;
- un blocker est rencontre ;
- la preuve reelle d'execution bornee manque ;
- le succes suppose une violation doctrinale ou une ouverture prematuree de KB-302 a KB-305.

## 10. Rollback minimal

Si la qualification echoue, le rollback minimal est le suivant :

- arreter immediatement la sequence de qualification ;
- stopper tout processus temporaire demarre pour la verification ;
- nettoyer les artefacts temporaires crees uniquement pour le test, sans toucher au socle canonique ;
- retirer toute configuration locale purement temporaire ajoutee pour la tentative ;
- conserver le registry et les documents phase 2 inchanges ;
- consigner le blocker rencontre et maintenir le verdict `non_integre` ;
- ne jamais deplacer le compute vers `db-layer` pour "sauver" la qualification.

Le rollback KB-301 doit etre minimal et local. Il ne doit ni casser le socle phase 2, ni provoquer une revision doctrinale.

## 11. Definition of done KB-301

KB-301 est `DONE` si et seulement si :

- le socle phase 2 reste fige et intact ;
- le role de `student` comme premier compute node local reel est explicite ;
- le role de `db-layer` comme control plane est explicitement protege ;
- un perimetre de preuve `student` borne est choisi ;
- tous les prerequis minimaux obligatoires passent ;
- au moins un launchable `student` standard a une preuve reelle d'execution bornee ;
- les preuves minimales sont collectees et relisibles humainement ;
- le verdict final est `integre` ;
- les limites de KB-301 par rapport a KB-302+ sont ecrites et respectees.

Corollaire :

- si tout reste documentaire, KB-301 n'est pas done ;
- si la reussite depend d'un cloud GPU, KB-301 n'est pas done ;
- si la reussite depend de services exposes ou d'un model serving reel, KB-301 n'est pas done ;
- si `db-layer` sert de compute standard, KB-301 n'est pas done.

## 12. Ce qui est explicitement laisse a KB-302+

Les sujets suivants ne doivent pas etre absorbes dans KB-301 :

- KB-302 : policy et exploitation des services exposables, y compris Jupyter/API/TensorBoard au-dela d'un simple controle loopback borne ;
- KB-303 : UI operateur, dashboard, lecture ergonomique des launchables et de leur etat ;
- KB-304 : cloud GPU effectif, comptes, secrets, SSH, policy cloud et integration externe ;
- KB-305 : inference runtime, model serving, endpoints GPU, vLLM et sujet serving en general ;
- orchestration complete, scheduling, multi-node, autoscaling ;
- pipeline complet de logs, observabilite avancee, reporting runtime ;
- refonte du control plane ou requalification de `db-layer`.

Frontiere stricte KB-301 :

- qualifier `student` comme premier compute node local reel ;
- ne rien absorber de plus pour obtenir ce verdict.
