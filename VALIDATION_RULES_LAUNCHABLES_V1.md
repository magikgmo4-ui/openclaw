# VALIDATION RULES LAUNCHABLES V1

## 1. Objectif

Figer les regles de validation statique V1 pour deux objets documentaires :
- un fichier Launchable OpenClaw au format YAML ;
- le fichier `launchables_registry.yaml`.

Le but est de rendre la doctrine Brev-like verifiable avant tout runtime.

## 2. Perimetre valide

Cette validation couvre :
- la forme minimale du Launchable ;
- les valeurs autorisees ;
- les compatibilites `profile -> node` ;
- les contraintes `node -> role` ;
- la coherence entre un Launchable et son entree de registry ;
- les restrictions de phase et de securite ;
- le cas special `db-layer`.

Cette validation ne couvre pas :
- l'existence runtime des images, chemins, comptes ou secrets ;
- l'execution reelle ;
- le bootstrap detaille ;
- la disponibilite reelle du cloud GPU.

## 3. Objets valides

### 3.1 Launchable YAML
Un Launchable valide doit avoir :
- `apiVersion: openclaw/v1`
- `kind: Launchable`
- `metadata`
- `spec`

### 3.2 Registry YAML
Le registry valide doit avoir :
- `registry_version`
- `registry_kind`
- `doctrine`
- `nodes`
- `launchables`

## 4. Launchable - champs obligatoires

Les champs suivants sont obligatoires :
- `apiVersion`
- `kind`
- `metadata.name`
- `spec.profile`
- `spec.agent.id`
- `spec.workspace.id`
- `spec.workspace.root`
- `spec.workspace.persistence`
- `spec.node.target`
- `spec.bootstrap.profile`
- `spec.policy.network.mode`
- `spec.policy.auth.mode`
- `spec.policy.sandbox.mode`
- `spec.policy.exposure.defaultDeny`
- `spec.services`

Pour chaque service actif dans `spec.services` :
- `name`
- `type`
- `enabled`
- `bind`
- `port`
- `visibility`
- `auth`

## 5. Launchable - champs optionnels autorises

Les champs optionnels reconnus en V1 sont :
- `metadata.description`
- `metadata.labels`
- `spec.agent.image`
- `spec.agent.entrypoint`
- `spec.workspace.mounts`
- `spec.bootstrap.preset`
- `spec.bootstrap.env`
- `spec.resources.cpu`
- `spec.resources.memory`
- `spec.resources.gpu`
- `spec.outputs.artifactsPath`
- `spec.outputs.logsPath`

## 6. Champs interdits ou non reconnus

En validation stricte V1, tout champ non liste dans la spec Launchable V1 ou dans le registry canonique est invalide.

Exemples de champs interdits en V1 :
- `spec.secrets`
- `spec.tolerations`
- `spec.scheduler`
- `spec.autoscaling`
- `spec.publicEndpoint`
- tout champ top-level inconnu

## 7. Contraintes de type simples

- les identifiants sont des strings non vides ;
- `spec.workspace.root` est une string representant un chemin absolu ;
- `spec.workspace.persistence` vaut `persistent` ou `ephemeral` ;
- `spec.resources.gpu` est un entier si present ;
- `spec.policy.exposure.defaultDeny` vaut `true` ;
- `spec.services` est une liste, vide autorisee ;
- `spec.bootstrap.env` et `metadata.labels` sont des maps si presents.

## 8. Contraintes de coherence internes au Launchable

- `spec.bootstrap.profile` doit etre identique a `spec.profile` ;
- `metadata.name` doit decrire le meme usage que `spec.workspace.id` ;
- `spec.node.target` doit etre un node autorise ;
- chaque `spec.services[].name` doit etre unique ;
- si un service est actif, `bind`, `port`, `visibility` et `auth` sont obligatoires ;
- si `visibility: loopback`, alors `bind` doit valoir `127.0.0.1` ;
- si `visibility: private-lan`, alors `spec.policy.network.mode` ne peut pas valoir `loopback-only` ;
- un service actif ne peut jamais porter `auth: none` ;
- `spec.resources.gpu` ne peut apparaitre que pour `training`, `vision` ou `inference` ;
- `spec.resources.gpu` doit etre absent ou egal a `0` sur `db-layer`.

## 9. Contraintes `profile -> node`

- `lab` -> `student` uniquement
- `vision` -> `student` uniquement
- `hf_build` -> `student` uniquement
- `training` -> `student` ou `cloud-gpu-future`
- `inference` -> `cloud-gpu-future` ou exception restreinte sur `db-layer`

Tout autre couple `profile/node` est invalide.

## 10. Contraintes `node -> role`

- `db-layer` doit avoir le role `control-plane`
- `db-layer` doit avoir `general_compute_allowed: false`
- `student` doit etre un target de `compute-local`
- `cloud-gpu-future` doit etre un target de `compute-external-future`

Un Launchable ne peut pas attribuer ou redefinir le role d'un node. Le role se lit dans le registry, jamais dans le Launchable lui-meme.

## 11. Contraintes `launchable -> registry`

Pour etre validable, un Launchable doit correspondre a exactement une entree de `launchables_registry.yaml`.

Les champs suivants doivent etre coherents entre le Launchable et le registry :
- `metadata.name` = `launchables[].name`
- `spec.profile` = `launchables[].profile`
- `spec.node.target` = `launchables[].node`
- chemin du fichier = `launchables[].source`

Le node reference par le Launchable doit exister dans `nodes`.

Le registry doit fournir pour l'entree correspondante :
- `id`
- `status`
- `allowed_in_phase`
- `phase_guard`
- `exception_class`
- `runtime_scope`
- `restricted_reason` si le statut est `restricted`

## 12. Contraintes de phase

- un Launchable ne peut etre traite en V1/phase 2 que si son entree de registry contient `phase-2-early` dans `allowed_in_phase` ;
- un Launchable avec `status: future` reste documentaire et ne constitue pas une preuve de readiness runtime ;
- un Launchable avec `status: restricted` doit etre lu comme exception gouvernee et non comme profil standard.

## 13. Regles speciales `db-layer`

### 13.1 Regle canonique
Tout Launchable pointant vers `db-layer` est interdit, sauf exception canonique unique.

### 13.2 Exception canonique unique
Un Launchable sur `db-layer` n'est acceptable que si toutes les conditions suivantes sont vraies :
- `spec.profile` vaut `inference` ;
- l'entree de registry existe ;
- `launchables[].status` vaut `restricted` ;
- `launchables[].phase_guard` vaut `control-plane-exception-only` ;
- `launchables[].exception_class` vaut `inference-local-validation` ;
- `launchables[].runtime_scope` vaut `local-validation-only` ;
- `launchables[].sensitivity` vaut `sensitive` ;
- `nodes.db-layer.general_compute_allowed` vaut `false` ;
- `nodes.db-layer.exception_policy.required` vaut `true` ;
- `nodes.db-layer.exception_policy.allowed_exception_classes` contient `inference-local-validation` ;
- `spec.policy.network.mode` vaut `loopback-only` ;
- tout service actif a `visibility: loopback` et `bind: 127.0.0.1` ;
- `spec.resources.gpu` est absent ou egal a `0`.

### 13.3 Consequence
Un Launchable `db-layer` normal, silencieux, ou non marque comme exception restreinte est invalide.

## 14. Conditions de classement

### VALIDE
Le fichier respecte toutes les regles obligatoires, est coherent avec le registry, et ne declenche aucune exception ni restriction speciale.

### VALIDE AVEC RESTRICTION
Le fichier respecte toutes les regles, mais son statut registry est `restricted` ou `future`.

### INVALIDE
Le fichier viole au moins une regle bloquante de structure, de compatibilite, de securite, de phase ou de doctrine infra.

## 15. Erreurs bloquantes

- `Invalid launchable: missing required field '<field>'`
- `Invalid launchable: unknown top-level field '<field>'`
- `Invalid launchable: unknown node '<node>'`
- `Invalid launchable: profile '<profile>' is incompatible with node '<node>'`
- `Invalid launchable: bootstrap profile does not match spec.profile`
- `Invalid launchable: service '<name>' is active without explicit auth`
- `Invalid launchable: service '<name>' violates loopback policy`
- `Invalid launchable: GPU allocation is not allowed on db-layer`
- `Invalid launchable: registry entry not found for source '<path>'`
- `Invalid launchable: registry mismatch on name/profile/node`
- `Invalid launchable: db-layer target is not marked as the allowed exception`
- `Invalid launchable: general compute is forbidden on control-plane node 'db-layer'`
- `Invalid registry: node 'db-layer' must declare role 'control-plane'`
- `Invalid registry: node 'db-layer' must set general_compute_allowed to false`
- `Invalid registry: restricted launchable on db-layer must declare exception_class, runtime_scope and sensitivity`

## 16. Sortie attendue d'un futur validateur

Un futur validateur peut produire, pour chaque fichier :
- `VALID`
- `VALID_WITH_RESTRICTION`
- `INVALID`

avec :
- la liste des regles verifiees ;
- la liste des erreurs bloquantes ;
- la liste des restrictions detectees.
