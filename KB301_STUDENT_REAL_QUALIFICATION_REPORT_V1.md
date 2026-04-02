# KB301 STUDENT REAL QUALIFICATION REPORT V1

## 1. Etat de depart repris

- Runbook KB-301 : `DONE` sur le plan documentaire.
- Verdict terrain initial avant retest : `non_integre`.
- Micro-chantier pre-retest acquis :
  - pathing canonique minimal `hf_build` en place sur `student`
  - runtime conteneur compatible prouve via `podman`
  - `vision` explicitement hors retest faute de GPU prouve
- Doctrine rappelee : `db-layer` reste control plane uniquement ; aucun compute standard n'y est reporte ; le seul cas de preuve autorise ici est `examples/launchable_hf_build_student.yaml` sur `student`.

## 2. Verifications reelles

### Node / acces

- acces `ssh student` reussi depuis `db-layer` ;
- identite relevee : `hostname=student`, `whoami=student` ;
- role canonique confirme par le registry : `nodes.student.role = compute-local`, `general_compute_allowed = true`.

### Registry / validation

- verification structure + coherence `launchable_hf_build_student.yaml` -> `launchables_registry.yaml` effectuee par parse YAML reel ;
- resultats : aucun echec ;
- points confirmes :
  - `apiVersion: openclaw/v1`
  - `kind: Launchable`
  - `metadata.name = hf-build-student`
  - `spec.profile = hf_build`
  - `spec.node.target = student`
  - `spec.bootstrap.profile = hf_build`
  - `spec.policy.network.mode = egress-controlled`
  - `spec.policy.sandbox.mode = container`
  - `spec.services = []`
  - `defaultDeny = true`
  - entree registry `launchable-hf-build-student-v1` trouvee
  - `phase-2-early` present
  - `phase_guard = standard-compute`
  - `runtime_scope = local-compute`

### Pathing / workspace

- chemins reels verifies et writables sur `student` :
  - `/srv/openclaw/workspaces/ws-hf-build-student`
  - `/srv/openclaw/outputs/ws-hf-build-student/artifacts`
  - `/srv/openclaw/outputs/ws-hf-build-student/logs`
- le pathing canonique minimal requis par le launchable est donc exploitable.

### Runtime conteneur

- runtime prouve sur `student` : `podman version 4.3.1` ;
- runtime OCI releve : `crun` ;
- execution reelle prouvee : `podman run --rm --pull=never docker.io/library/alpine:3.20 true` ;
- reexecution bornee reussie avec montages workspace / artifacts / logs ;
- image declaree dans le launchable `openclaw/hf-build-worker:v1` absente localement au moment du retest.

### Bootstrap minimal

- bootstrap minimal soutenable sans autre chantier :
  - preset documentaire : `hf-build-base`
  - env non secretes injectables : `OPENCLAW_MODE=hf_build`, `CACHE_POLICY=workspace-local`
  - sandbox `container` soutenable via `podman`
  - aucun service a demarrer (`services: []`)
  - aucune ouverture publique, aucun tunnel, aucun cloud, aucun secours `db-layer`

### Execution bornee `hf_build`

- execution reelle realisee sur `student` avec un runner conteneur minimal deja present : `docker.io/library/alpine:3.20` ;
- commande borne executee via `podman`, avec montages sur les chemins canoniques du launchable et injection des env du launchable ;
- sortie observee : `hf_build_probe_ok` puis `exit_code=0` ;
- seconde execution de verification reussie egalement ;
- aucun service expose, aucun port ouvert, aucun detour par `db-layer` compute.

### Traces / artefacts / logs minimaux

- preuves creees sur `student` :
  - `/srv/openclaw/workspaces/ws-hf-build-student/kb301_hf_build_status.txt`
  - `/srv/openclaw/workspaces/ws-hf-build-student/cache/kb301_hf_build_cache_state.txt`
  - `/srv/openclaw/outputs/ws-hf-build-student/artifacts/kb301_hf_build_probe.txt`
  - `/srv/openclaw/outputs/ws-hf-build-student/logs/kb301_hf_build_probe.log`
- contenus verifies :
  - statut : `bounded_execution=ok`
  - cache workspace : `workspace_cache_ready`
  - artefact : `node=student`, `profile=hf_build`, `mode=hf_build`, `cache_policy=workspace-local`, timestamp present
  - log : `bootstrap_preset=hf-build-base`, chemins de travail, `artifact_created`, `workspace_cache_ready`, `status_written`
- ces traces satisfont le minimum KB-301 : exit code observable, log local, artefact local, pathing de logs et d'artefacts reel.

## 3. Resultats observes

### Ce qui est prouve

- identite node claire et accessibilite reelle de `student` ;
- coherence documentaire du launchable `hf_build` avec le registry et les regles de validation ;
- pathing canonique minimal reel et writable ;
- runtime conteneur reel utilisable sur `student` ;
- bootstrap minimal soutenable sans ouvrir un autre chantier ;
- execution bornee reelle compatible `hf_build` ;
- traces minimales observables et collectees ;
- qualification restee strictement dans KB-301, sans derive vers `db-layer` ou KB-302+.

### Ce qui reste non prouve

- presence locale de l'image declaree `openclaw/hf-build-worker:v1` ;
- semantique applicative complete d'un vrai worker OpenClaw `hf_build` ;
- tout autre profil (`lab`, `vision`, `training`).

### Ce qui est explicitement hors perimetre

- `vision` ;
- `lab` comme cas de compensation ;
- `training` ;
- KB-302, KB-303, KB-304, KB-305 ;
- tout compute standard sur `db-layer`.

## 4. Verdict KB-301

`integre`

- `student` est identifiable, joignable et opere comme node distinct de `db-layer`.
- Le launchable `hf_build` reste coherent avec le registry, KB-202, KB-203, KB-204 et KB-205.
- Le pathing canonique minimal attendu par `hf_build` existe et est writable sur `student`.
- Le runtime conteneur requis pour le mode `container` est prouve en reel via `podman`.
- Le bootstrap minimal `hf_build` est soutenable sans ouvrir KB-302+.
- Une execution bornee reelle a ete demontree sur `student` avec exit code, artefact et log observables.
- Aucune derive doctrinale n'a ete necessaire : pas de secours compute sur `db-layer`, pas de cloud, pas de service public.

## 5. Definition of done

- KB-301 est maintenant `DONE` sur le plan de la qualification reelle du node `student`.
- La definition of done du runbook est satisfaite dans le perimetre borne retenu :
  - `student` est qualifie comme premier compute node local reel ;
  - le socle phase 2 reste intact ;
  - la preuve canonique minimale repose sur un seul cas `hf_build` ;
  - des traces minimales reelles existent ;
  - aucune derive vers KB-302+ n'a ete necessaire.

## 6. Point de reprise suivant

- Plus petit prochain pas utile apres KB-301 : consigner `student` comme node compute local standard qualifie et garder `examples/launchable_hf_build_student.yaml` comme probe minimal de reference.
- Aucun autre chantier n'a besoin d'etre ouvert automatiquement.
- Si un besoin concret apparait ensuite, le prochain chantier candidat serait KB-302, mais seulement sur demande explicite et sans reouvrir KB-301.
