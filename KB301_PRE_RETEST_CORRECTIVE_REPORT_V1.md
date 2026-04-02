# KB301 PRE RETEST CORRECTIVE REPORT V1

## 1. Etat de depart repris

- KB-301 runbook : `DONE`.
- Qualification reelle KB-301 : executee.
- Verdict terrain initial : `non_integre`.
- Blockers retenus comme canoniques pour ce micro-chantier :
  1. pathing canonique absent ou incomplet sous `/srv/openclaw*`
  2. aucun runtime conteneur prouve sur `student`
  3. aucun GPU prouve pour `vision` telle qu'ecrite
- Doctrine rappelee : `db-layer` reste control plane uniquement ; aucun compute standard n'y est reporte ; le seul node reel vise par ce micro-chantier est `student`.
- Terrain reel inspecte via `ssh student` depuis `db-layer`, sans transferer de compute sur `db-layer`.

## 2. Diagnostic reel par blocker

### Pathing canonique

- Sur `student`, les bases canoniques existent deja :
  - `/srv/openclaw`
  - `/srv/openclaw/workspaces`
  - `/srv/openclaw/outputs`
- L'ecart reel n'est donc pas un absent total de `/srv/openclaw`.
- L'ecart reel est plus petit : les sous-chemins attendus par les launchables ne sont pas presents.
- Constat utile pour le retest minimal :
  - absent avant correctif : `/srv/openclaw/workspaces/ws-hf-build-student`
  - absent avant correctif : `/srv/openclaw/outputs/ws-hf-build-student/artifacts`
  - absent avant correctif : `/srv/openclaw/outputs/ws-hf-build-student/logs`
- Le plus petit correctif utile est donc de materialiser seulement le pathing `hf_build` de base, pas toute une arborescence speculative.

### Runtime conteneur

- `docker` n'a pas ete prouve sur `student`.
- `podman` est present sur `student` : `podman version 4.3.1`.
- Le runtime OCI `crun` est visible via `podman info`.
- Une image locale minimale existe deja : `docker.io/library/alpine:3.20`.
- Preuve reelle obtenue sans chantier large :
  - `podman run --rm --pull=never docker.io/library/alpine:3.20 true` reussit ;
  - un conteneur `podman` a aussi monte les chemins `hf_build` canoniques et y a ecrit/retire des probes avec succes.
- Conclusion : oui, un runtime conteneur compatible peut etre prouve sur `student` sans ouvrir un chantier large. Le plus petit chemin utile est `podman`, pas une installation supplementaire.

### Statut GPU / `vision`

- `nvidia-smi` est absent sur `student`.
- Aucun GPU NVIDIA ou compute GPU explicite n'est prouve.
- Observation reelle : `lspci` montre seulement `Intel Corporation HD Graphics 530`.
- Observation reelle : `/dev/dri/card0` et `/dev/dri/renderD128` existent, mais cela ne prouve pas un GPU compute compatible avec `vision` telle qu'ecrite (`gpu: 1`).
- Conclusion canonique : `vision` ne doit pas etre requalifiee de force.
- Pour ce micro-chantier, `vision` doit etre marquee `hors retest KB-301` faute de GPU prouve.

## 3. Correctifs minimaux appliques

### Correctifs reels appliques sur `student`

- creation des seuls chemins necessaires au cas de base `hf_build` :
  - `/srv/openclaw/workspaces/ws-hf-build-student`
  - `/srv/openclaw/outputs/ws-hf-build-student/artifacts`
  - `/srv/openclaw/outputs/ws-hf-build-student/logs`

### Commandes utiles utilisees

- diagnostic pathing :
  - `ssh student "ls -ld /srv /srv/openclaw /srv/openclaw/workspaces /srv/openclaw/outputs"`
- creation minimale du pathing `hf_build` :
  - `ssh student "mkdir -p /srv/openclaw/workspaces/ws-hf-build-student /srv/openclaw/outputs/ws-hf-build-student/artifacts /srv/openclaw/outputs/ws-hf-build-student/logs"`
- preuve runtime conteneur minimal :
  - `ssh student "podman run --rm --pull=never docker.io/library/alpine:3.20 true"`
- preuve runtime + pathing monte/writable :
  - `ssh student "podman run --rm --pull=never -v /srv/openclaw/workspaces/ws-hf-build-student:/workspace:Z -v /srv/openclaw/outputs/ws-hf-build-student/artifacts:/artifacts:Z -v /srv/openclaw/outputs/ws-hf-build-student/logs:/logs:Z docker.io/library/alpine:3.20 sh -c 'touch /workspace/.probe && touch /artifacts/.probe && touch /logs/.probe && rm /workspace/.probe /artifacts/.probe /logs/.probe'"`

### Ce qui n'a volontairement pas ete touche

- aucun chemin `lab` ou `vision` n'a ete provisionne ;
- aucun service Jupyter/API/TensorBoard n'a ete ouvert ;
- aucun tunnel public ;
- aucune installation Docker, Kubernetes, compose ou orchestration ;
- aucun chantier GPU ;
- aucune modification sur `db-layer` hors simple point de rebond SSH ;
- aucune reecriture phase 2 ou KB-302+.

## 4. Verifications reelles apres correctif

### Ce qui est maintenant prouve

- `student` est joignable et identifiable comme node distinct ;
- le pathing minimal `hf_build` canonique existe sur `student` ;
- ces chemins sont writables par l'operateur `student` ;
- un runtime conteneur compatible existe reellement sur `student` via `podman` + `crun` ;
- un conteneur local peut s'executer sans pull et monter/ecrire sur les chemins `hf_build` canoniques ;
- le micro-chantier n'a pas derive vers `db-layer` compute ni vers KB-302+.

### Ce qui reste non prouve, sans bloquer le retest `hf_build`

- un GPU compute local utilisable pour `vision` ;
- l'execution loopback d'un service `lab` type notebook ;
- un profil `training` sur `student` ;
- une image applicative OpenClaw ciblee autre que l'image minimale de preuve conteneur.

### Ce qui reste explicitement hors perimetre

- services exposables KB-302 ;
- UI operateur KB-303 ;
- cloud GPU KB-304 ;
- inference / model serving KB-305 ;
- tout fallback compute sur `db-layer`.

## 5. Impact par profil

### `hf_build`

- devient le cas de base minimal recommande pour relancer KB-301 ;
- raison : pas de service expose, pas de prerequis GPU, sandbox `container` maintenant prouve, pathing canonique minimal en place ;
- c'est le profil le plus propre pour un retest borne et credible.

### `lab`

- reste sous reserve ;
- raison : ce micro-chantier n'a pas provisionne ses chemins dedies et n'a pas rejoue le sujet service loopback `jupyter` ;
- le profil n'est pas invalide, mais il n'est pas le meilleur point de reprise minimal tant que KB-302 reste ferme.

### `vision`

- doit etre traitee `hors retest KB-301` ;
- raison : aucun GPU compute n'est prouve pour soutenir honnêtement `resources.gpu: 1` ;
- aucune requalification artificielle n'est faite.

### `training`

- statut prudent uniquement ;
- aucun launchable `training` sur `student` n'est actuellement enregistre dans le registry ;
- ce profil ne participe pas au retest minimal.

## 6. Verdict micro-chantier

`retest_kb301_autorise`

- Le blocker de pathing est leve au niveau minimal utile pour `hf_build`.
- Un runtime conteneur compatible est maintenant prouve factuellement sur `student`.
- `vision` est traitee proprement par exclusion du retest, sans faux OK GPU.
- `hf_build` devient un cas de base credible pour relancer la qualification reelle.
- `lab` reste secondaire et sous reserve, sans bloquer le retest minimal.
- Aucun elargissement vers KB-302, KB-303, KB-304 ou KB-305 n'a ete necessaire.
- `db-layer` reste strictement hors compute standard.

## 7. Point de reprise suivant

- Plus petit prochain pas utile : relancer la qualification reelle KB-301 sur `student` en prenant `examples/launchable_hf_build_student.yaml` comme seul cas de base.
- `vision` doit etre explicitement exclue de ce retest.
- `lab` peut rester en option secondaire seulement si `hf_build` passe et sans ouvrir KB-302.
