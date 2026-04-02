# SPEC LAUNCHABLE OPENCLAW YAML V1

## 1. Objet

Cette spec definit le format canonique V1 d'un Launchable OpenClaw. Elle couvre la structure YAML, les champs requis, les champs optionnels, les valeurs autorisees et les regles minimales de validation.

## 2. Principe

Un Launchable decrit une unite declarative reproductible de lancement. Il ne decrit pas le runtime detaille, mais il doit permettre de figer sans ambiguite :
- le profil d'usage ;
- l'agent vise ;
- le workspace ;
- le node cible ;
- le bootstrap ;
- la policy ;
- les services eventuels.

## 3. Structure canonique

```yaml
apiVersion: openclaw/v1
kind: Launchable
metadata:
  name: lab-student
  description: Session lab locale sur student
spec:
  profile: lab
  agent:
    id: assistant-lab
  workspace:
    id: ws-lab-student
    root: /srv/openclaw/workspaces/ws-lab-student
    persistence: persistent
  node:
    target: student
  bootstrap:
    profile: lab
    preset: base-python
  policy:
    network:
      mode: loopback-only
    auth:
      mode: local-user
    sandbox:
      mode: process
    exposure:
      defaultDeny: true
  services:
    - name: notebook
      type: jupyter
      enabled: true
      bind: 127.0.0.1
      port: 8888
      visibility: loopback
      auth: token
```

## 4. Champs obligatoires

Les champs suivants sont obligatoires :

| Champ | Type | Regle |
|---|---|---|
| `apiVersion` | string | doit valoir `openclaw/v1` |
| `kind` | string | doit valoir `Launchable` |
| `metadata.name` | string | slug stable, unique dans le repository |
| `spec.profile` | string | voir valeurs autorisees |
| `spec.agent.id` | string | identifiant operatoire de l'agent |
| `spec.workspace.id` | string | identifiant stable du workspace |
| `spec.workspace.root` | string | chemin absolu du workspace cible |
| `spec.workspace.persistence` | string | `persistent` ou `ephemeral` |
| `spec.node.target` | string | `db-layer`, `student`, `cloud-gpu-future` |
| `spec.bootstrap.profile` | string | profil de bootstrap aligne sur l'usage |
| `spec.policy.network.mode` | string | voir valeurs autorisees |
| `spec.policy.auth.mode` | string | voir valeurs autorisees |
| `spec.policy.sandbox.mode` | string | voir valeurs autorisees |
| `spec.policy.exposure.defaultDeny` | boolean | doit etre `true` en V1 |
| `spec.services` | list | liste explicite, vide autorisee |

## 5. Champs optionnels

| Champ | Type | Usage |
|---|---|---|
| `metadata.description` | string | rappel humain compact |
| `metadata.labels` | map | labels techniques de tri |
| `spec.agent.image` | string | image ou reference d'execution si deja connue |
| `spec.agent.entrypoint` | string | commande ou point d'entree declaratif |
| `spec.workspace.mounts` | list | montages supplementaires controles |
| `spec.bootstrap.preset` | string | preset de bootstrap minimal |
| `spec.bootstrap.env` | map | variables de bootstrap non secretes |
| `spec.resources.cpu` | string | borne ou reservation CPU declarative |
| `spec.resources.memory` | string | borne ou reservation memoire declarative |
| `spec.resources.gpu` | integer | nombre de GPU demandes |
| `spec.outputs.artifactsPath` | string | repertoire cible des artefacts |
| `spec.outputs.logsPath` | string | repertoire cible des logs |

## 6. Valeurs autorisees

### `spec.profile`
- `lab`
- `training`
- `inference`
- `vision`
- `hf_build`

### `spec.workspace.persistence`
- `persistent`
- `ephemeral`

### `spec.node.target`
- `db-layer`
- `student`
- `cloud-gpu-future`

### `spec.policy.network.mode`
- `loopback-only` : services limites a `127.0.0.1`
- `private-lan` : reserve a un reseau prive controle, jamais par defaut
- `egress-controlled` : sortie reseau autorisee selon liste de controle, sans exposition entrante automatique

### `spec.policy.auth.mode`
- `local-user` : execution rattachee a l'operateur local ou a un compte technique local
- `token` : service protege par jeton unique
- `ssh-key` : acces restreint par cle pour cas explicitement gouvernes

### `spec.policy.sandbox.mode`
- `process`
- `container`
- `restricted-container`

### `spec.services[].type`
- `jupyter`
- `http-api`
- `tensorboard`

### `spec.services[].visibility`
- `loopback`
- `private-lan`
- `disabled`

### `spec.services[].auth`
- `token`
- `local-user`
- `ssh-key`
- `none` uniquement si `enabled: false`

## 7. Regles de validation

### Regles generales
- `metadata.name` et `spec.workspace.id` doivent etre stables et porter le meme usage fonctionnel.
- `spec.bootstrap.profile` doit etre identique a `spec.profile`.
- `spec.workspace.root` doit etre un chemin absolu.
- `spec.services` doit toujours exister, meme si la liste est vide.
- chaque service doit avoir un `name` unique dans le fichier.

### Regles de compatibilite profil -> node
- `lab` : `student` uniquement en V1.
- `vision` : `student` uniquement en V1.
- `hf_build` : `student` uniquement en V1.
- `training` : `student` ou `cloud-gpu-future`.
- `inference` : `db-layer` pour validation locale bornee, ou `cloud-gpu-future` pour extension future.

### Regles de securite minimales
- Si `spec.services[].enabled` vaut `true`, alors `bind` est requis.
- Si `visibility: loopback`, alors `bind` doit valoir `127.0.0.1`.
- Si `visibility: private-lan`, alors `spec.policy.network.mode` ne peut pas etre `loopback-only`.
- Aucun service actif ne peut declarer `auth: none`.
- `spec.policy.exposure.defaultDeny` doit rester `true` sur tous les launchables V1.

### Regles de ressources
- `spec.resources.gpu` ne doit etre defini que pour `training`, `vision` ou `inference`.
- `spec.resources.gpu` doit etre absent ou egal a `0` sur `db-layer` en V1.

## 8. Erreurs de configuration a eviter

- Utiliser `db-layer` pour un profil `lab`, `vision` ou `hf_build`.
- Declarer un service sans policy explicite.
- Ouvrir `0.0.0.0` tout en laissant `loopback-only`.
- Rendre un notebook ou une API anonyme.
- Introduire des champs non documentes dans la spec.
- Confondre `workspace.id` logique et chemin physique du workspace.
- Declarer du GPU sur `db-layer` ou sur un profil qui n'en a pas besoin.

## 9. Mini exemple inline

```yaml
apiVersion: openclaw/v1
kind: Launchable
metadata:
  name: inference-local-db-layer
spec:
  profile: inference
  agent:
    id: local-inference-agent
  workspace:
    id: ws-inference-local
    root: /srv/openclaw/workspaces/ws-inference-local
    persistence: persistent
  node:
    target: db-layer
  bootstrap:
    profile: inference
    preset: api-minimal
  policy:
    network:
      mode: loopback-only
    auth:
      mode: token
    sandbox:
      mode: restricted-container
    exposure:
      defaultDeny: true
  services:
    - name: inference-api
      type: http-api
      enabled: true
      bind: 127.0.0.1
      port: 8080
      visibility: loopback
      auth: token
```

## 10. Convention V1

La V1 reste volontairement compacte. Tout ce qui releve du scheduling detaille, des secrets, de l'auto-scaling, du lifecycle avance ou du registry ne fait pas partie de ce document et devra etre defini dans des specs separees.
