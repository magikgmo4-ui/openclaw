# KB302 LAB JUPYTER CLASSIFICATION REPORT V1

## 1. Etat de depart repris

- acquis retenus : phase 2 socle `CLOSE`, KB-301 `DONE`, `student` integre comme premier compute node local reel, KB-302 policy `DONE` ;
- doctrine retenue : `db-layer` reste control plane uniquement ; aucun compute standard ni service standard n'y est derive ;
- perimetre de classification : un seul cas, `examples/launchable_lab_student.yaml`, service `jupyter` loopback sur `student` ;
- hors perimetre de cette passe : deploiement reel du service, LAN controle, UI operateur, cloud GPU, inference / model serving.

## 2. Verifications reelles / documentaires

### Launchable `lab`

- `metadata.name = lab-student` ;
- `spec.profile = lab` ;
- `spec.node.target = student` ;
- `spec.bootstrap.profile = lab` ;
- `spec.policy.network.mode = loopback-only` ;
- `spec.policy.auth.mode = local-user` ;
- `spec.policy.sandbox.mode = process` ;
- `spec.policy.exposure.defaultDeny = true`.

### Registry

- entree registry trouvee : `launchable-lab-student-v1` ;
- coherence verifiee sur `name/profile/node/source` ;
- `phase-2-early` present ;
- `phase_guard = standard-compute` ;
- `runtime_scope = local-compute` ;
- `student` reste `compute-local` avec `general_compute_allowed = true`.

### Validation statique

- verification documentaire parsee : aucun echec ;
- le couple `lab -> student` est autorise par KB-202 ;
- le service actif declare tous les champs obligatoires : `name`, `type`, `enabled`, `bind`, `port`, `visibility`, `auth` ;
- `visibility: loopback` est coherent avec `bind: 127.0.0.1` ;
- `auth: token` sur le service actif respecte la regle de non-anonymat.

### Type de service

- le service declare est `type: jupyter` ;
- `jupyter` est un type reconnu par la spec V1 ;
- KB-302 classe explicitement `jupyter` loopback sur `lab` comme cas canonique `autorisable` sur `student`.

### Reseau

- lecture reseau strictement compatible avec `local-loopback-only` ;
- `network.mode = loopback-only` ;
- `visibility = loopback` ;
- `bind = 127.0.0.1` ;
- aucun signal LAN, Internet, tunnel public ou bind large.

### Auth

- auth du service : `token` ;
- auth du launchable : `local-user` ;
- pour ce cas, la combinaison est suffisante selon KB-302 : service loopback borne, attache a l'operateur local, sans partage public.

### Sandbox / isolation

- `sandbox.mode = process` ;
- KB-302 l'accepte pour un service local simple, loopback-only, faible risque, attache a un seul operateur ;
- ce cas reste dans cette lecture : un notebook unique, local, tokenise, sans LAN ni public ;
- aucune derive documentaire vers secrets control plane, socket Docker ou privilege eleve n'apparait dans le launchable.

### Frontiere de chantier

- le cas reste sur `student` ;
- il n'ouvre ni KB-303, ni KB-304, ni KB-305 ;
- il ne transforme pas `db-layer` et ne cree aucune surface standard sur le control plane ;
- il reste un service de session locale, pas une UI operateur ni un endpoint de serving.

## 3. Verdict de classification

`autorisable`

- le launchable `lab` est coherent avec le registry et passe la lecture KB-202 sans blocker ;
- `jupyter` est un type reconnu par la spec et explicitement prevu par KB-302 pour `lab` sur `student` ;
- la lecture reseau est strictement `local-loopback-only` avec `127.0.0.1` et `defaultDeny` ;
- l'auth de service par `token` est suffisante pour ce cas borne ;
- le mode `process` reste acceptable ici car le service est local, simple, non partage et non public ;
- aucune derive vers `db-layer`, KB-303, KB-304 ou KB-305 n'est necessaire ;
- le cas est donc classable positivement sans ouvrir un chantier large.

## 4. Reserves ou conditions exactes

- ce verdict classe la policy du cas ; il ne vaut pas ouverture runtime effective du service ;
- toute demande de passage en `private-lan` obligerait une reclassification en `autorisable_sous_conditions` avec revue explicite ;
- toute evolution vers multi-utilisateur, dashboard operateur ou front partage sortirait de KB-302 ;
- toute evolution vers API de prediction, inference exposee ou serving ferait sortir le cas vers KB-305 ;
- lors d'une future ouverture reelle, il faudra seulement verifier les prerequis operatoires minimaux du launchable sans elargir le chantier.

## 5. Point de reprise suivant

- plus petit prochain pas utile : si un besoin concret existe, ouvrir un micro-chantier strictement borne pour un runbook d'ouverture locale `jupyter` loopback sur `student`, pour ce seul launchable `lab-student` ;
- ne rien ouvrir d'autre automatiquement ;
- ne pas etendre au LAN, a `vision`, a `hf_build`, ni a une UI operateur tant qu'une demande explicite ne l'impose pas.
