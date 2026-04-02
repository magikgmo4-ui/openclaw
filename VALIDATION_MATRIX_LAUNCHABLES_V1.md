# VALIDATION MATRIX LAUNCHABLES V1

| Rule ID | Objet | Condition | Resultat attendu | Severite | Note |
|---|---|---|---|---|---|
| VR-001 | Launchable | `apiVersion` absent ou != `openclaw/v1` | INVALID | critique | version canonique obligatoire |
| VR-002 | Launchable | `kind` absent ou != `Launchable` | INVALID | critique | type canonique obligatoire |
| VR-003 | Launchable | un champ obligatoire manque | INVALID | critique | voir liste des champs requis |
| VR-004 | Launchable | champ non reconnu en validation stricte | INVALID | majeure | evite la derive silencieuse |
| VR-005 | Launchable | `spec.bootstrap.profile` != `spec.profile` | INVALID | critique | coherence interne minimale |
| VR-006 | Launchable | `spec.workspace.root` n'est pas absolu | INVALID | majeure | garde-fou documentaire |
| VR-007 | Launchable | `spec.services` absent | INVALID | critique | liste vide autorisee, absence interdite |
| VR-008 | Launchable | service actif sans `bind`/`port`/`visibility`/`auth` | INVALID | critique | policy explicite obligatoire |
| VR-009 | Launchable | service actif avec `auth: none` | INVALID | critique | interdit par security policy |
| VR-010 | Launchable | `visibility: loopback` et `bind` != `127.0.0.1` | INVALID | critique | loopback strict |
| VR-011 | Launchable | `visibility: private-lan` avec `network.mode: loopback-only` | INVALID | majeure | contradiction reseau |
| VR-012 | Launchable | `spec.policy.exposure.defaultDeny` != `true` | INVALID | critique | V1 default deny |
| VR-013 | Launchable | `lab` cible un node autre que `student` | INVALID | critique | compatibilite profil/node |
| VR-014 | Launchable | `vision` cible un node autre que `student` | INVALID | critique | compatibilite profil/node |
| VR-015 | Launchable | `hf_build` cible un node autre que `student` | INVALID | critique | compatibilite profil/node |
| VR-016 | Launchable | `training` cible un node hors `student`, `cloud-gpu-future` | INVALID | critique | compatibilite profil/node |
| VR-017 | Launchable | `inference` cible un node hors `db-layer`, `cloud-gpu-future` | INVALID | critique | compatibilite profil/node |
| VR-018 | Launchable | GPU declare pour un profil hors `training`, `vision`, `inference` | INVALID | majeure | usage GPU borne |
| VR-019 | Launchable | GPU > 0 sur `db-layer` | INVALID | critique | compute GPU interdit sur control plane |
| VR-020 | Registry | `registry_version` absent | INVALID | critique | format registry incomplet |
| VR-021 | Registry | `nodes.db-layer.role` != `control-plane` | INVALID | critique | doctrine infra canonique |
| VR-022 | Registry | `nodes.db-layer.general_compute_allowed` != `false` | INVALID | critique | verrou anti-derive |
| VR-023 | Registry | `nodes.student.general_compute_allowed` != `true` | INVALID | majeure | compute local normal |
| VR-024 | Registry | `nodes.cloud-gpu-future.general_compute_allowed` != `true` | INVALID | majeure | cible compute future |
| VR-025 | Registry | entree launchable sans `id`/`name`/`profile`/`node`/`status`/`source` | INVALID | critique | identite minimale registry |
| VR-026 | Link Launchable->Registry | aucun enregistrement registry ne correspond au fichier source | INVALID | critique | source canonique absente |
| VR-027 | Link Launchable->Registry | mismatch entre `metadata.name` et `launchables[].name` | INVALID | critique | nom canonique incoherent |
| VR-028 | Link Launchable->Registry | mismatch entre `spec.profile` et `launchables[].profile` | INVALID | critique | profil incoherent |
| VR-029 | Link Launchable->Registry | mismatch entre `spec.node.target` et `launchables[].node` | INVALID | critique | target incoherente |
| VR-030 | Link Launchable->Registry | `phase-2-early` absent de `allowed_in_phase` | INVALID | majeure | hors perimetre KB-202 |
| VR-031 | Link Launchable->Registry | `status: future` | VALID_WITH_RESTRICTION | mineure | documentaire, non runtime-ready |
| VR-032 | Link Launchable->Registry | `status: restricted` | VALID_WITH_RESTRICTION | mineure | exception gouvernee |
| VR-033 | db-layer | launchable cible `db-layer` avec profil != `inference` | INVALID | critique | compute generaliste interdit |
| VR-034 | db-layer | launchable cible `db-layer` sans `status: restricted` au registry | INVALID | critique | exception non marquee |
| VR-035 | db-layer | launchable cible `db-layer` sans `phase_guard: control-plane-exception-only` | INVALID | critique | garde de phase obligatoire |
| VR-036 | db-layer | launchable cible `db-layer` sans `exception_class: inference-local-validation` | INVALID | critique | exception canonique unique |
| VR-037 | db-layer | launchable cible `db-layer` sans `runtime_scope: local-validation-only` | INVALID | critique | usage borne obligatoire |
| VR-038 | db-layer | launchable cible `db-layer` sans `sensitivity: sensitive` | INVALID | majeure | cas sensible a marquer |
| VR-039 | db-layer | launchable cible `db-layer` avec `network.mode` != `loopback-only` | INVALID | critique | control plane non exposable par defaut |
| VR-040 | db-layer | service actif sur `db-layer` avec visibilite != `loopback` | INVALID | critique | exception locale seulement |
| VR-041 | db-layer | registry ne declare pas l'exception policy pour `db-layer` | INVALID | critique | verrou structurel absent |
| VR-042 | student | launchable `lab`, `vision` ou `hf_build` sur `student` avec policy valide | VALID | normale | compute local standard |
| VR-043 | cloud-gpu-future | launchable `training` ou `inference` avec `status: future` | VALID_WITH_RESTRICTION | normale | cible future documentaire |
| VR-044 | Launchable | `services: []` avec policy complete | VALID | normale | cas sans service expose |
| VR-045 | Launchable | service type hors `jupyter`, `http-api`, `tensorboard` | INVALID | majeure | types exposes bornes |

## Lecture courte

- `INVALID` bloque la validation statique.
- `VALID_WITH_RESTRICTION` autorise le maintien documentaire mais interdit de le lire comme compute standard.
- `db-layer` ne passe jamais en `VALID` standard : il passe uniquement en `VALID_WITH_RESTRICTION` si toute l'exception canonique est satisfaite.
