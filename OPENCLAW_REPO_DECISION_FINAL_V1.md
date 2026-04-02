# OPENCLAW REPO DECISION FINAL V1

## 1. Etat de depart repris

- La revalidation precedente concluait `repo_dedie_pas_encore_recommande`.
- Les deux ecarts bloquants identifies etaient `KANBAN_OPENCLAW.md` et `README.md`.
- Ces deux documents existent maintenant.
- Les documents minimaux demandes par le workflow sont tous presents : `WORKFLOW_OPENCLAW.md`, `REPO_BOUNDARIES_OPENCLAW.md`, `ETABLI_OPENCLAW.md`, `SESSION_OPENING_INDEX.md`, `KANBAN_OPENCLAW.md`, `README.md`.
- OpenClaw reste borne en mode doc/gouvernance-only au demarrage.

## 2. Ecarts desormais fermes

- `KANBAN_OPENCLAW.md` ferme l ecart de visualisation canonique de l etat des chantiers.
- `README.md` ferme l ecart de point d entree rapide du futur repo.
- Le socle documentaire minimal est maintenant complet sans dependance a une memoire externe diffuse.

## 3. Conditions minimales maintenant remplies

Les conditions minimales ecrites dans `WORKFLOW_OPENCLAW.md` pour recommander un repo dedie sont maintenant remplies :

- `WORKFLOW_OPENCLAW.md` existe et est exploitable ;
- `REPO_BOUNDARIES_OPENCLAW.md` borne le perimetre control/compute et le hors-repo ;
- `ETABLI_OPENCLAW.md` fixe l etat acquis, les limites et les non-etablis ;
- `SESSION_OPENING_INDEX.md` donne un point d entree minimal de session ;
- `KANBAN_OPENCLAW.md` visualise l etat des chantiers ;
- `README.md` pointe l entree du projet.

En plus de ces minima :

- le perimetre doc/gouvernance-only est ecrit ;
- la separation avec `opt-trading` et `localcms` est ecrite ;
- la doctrine `db-layer` / `student` / `cloud-gpu-future` est ecrite ;
- le socle canonique spec/validation/runbooks existe deja.

## 4. Manques restants reels

- Aucun manque bloquant supplementaire n apparait dans le canon relu.
- Des limites runtime subsistent, mais elles etaient deja connues, explicitement documentees, et ne font pas partie des preconditions minimales de creation du repo documentaire.
- Aucun nouveau prerequis ne doit etre ajoute artificiellement a ce stade.

## 5. Verdict final

`repo_dedie_recommande`

Motif net :

- le socle minimal de gouvernance demande par le workflow est maintenant complet ;
- le perimetre du futur repo est suffisamment clair, borne et sain ;
- le repo peut demarrer comme repo doc/gouvernance-only sans confusion majeure, puisque ses limites sont explicitement ecrites.

## 6. Conditions avant creation effective du repo

Le verdict `repo_dedie_recommande` n autorise pas une creation confuse ou derivee. Avant creation effective du repo GitHub, il faut conserver les regles suivantes :

- creer un repo strictement doc/gouvernance-only ;
- ne pas y mettre de code runtime, secrets, logs, artefacts ou configurations machine ;
- ne pas melanger OpenClaw avec `opt-trading` ou `localcms` ;
- garder `db-layer` en control plane uniquement ;
- ne pas presenter `cloud-gpu-future` comme actif ;
- ne pas rouvrir KB-301 ni KB-302 par effet de bord de la creation du repo.

## 7. Plus petit prochain pas utile

- Si la decision doit etre executee, creer ensuite le repo GitHub dedie avec ce perimetre documentaire strict.
- Si la creation n est pas executee immediatement, conserver ce verdict comme reference canonique de gouvernance.
- Dans les deux cas, ne lancer ni migration Git ni deplacement de code sans mission explicite distincte.
