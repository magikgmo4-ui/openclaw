# KB302 LAB JUPYTER LOOPBACK RUNBOOK V1

## 1. But du micro-chantier

Ouvrir un seul service local reel, borne et reversible : `lab` + `jupyter` sur `student`, en `loopback-only`.

Ce micro-chantier existe pour operationaliser le seul cas deja classe `autorisable`, sans ouvrir un deploiement large, sans LAN, sans public, sans second service, et sans derive hors KB-302.

## 2. Socle canonique rappele

- phase 2 documentaire socle = `CLOSE` ;
- KB-301 = `DONE` ;
- `student` = premier compute node local reel integre ;
- KB-302 policy = `DONE` ;
- `lab` + `jupyter` loopback sur `student` = `autorisable` ;
- `db-layer` reste control plane uniquement ; aucun service standard n'y est reporte.

## 3. Perimetre exact

Le present runbook couvre seulement :

- le Launchable `examples/launchable_lab_student.yaml` ;
- le service unique `notebook` de type `jupyter` ;
- le node `student` ;
- un bind strict `127.0.0.1` ;
- une auth par token ;
- une ouverture locale bornee avec traces minimales et rollback minimal.

Le present runbook ne couvre pas :

- LAN, Internet, tunnel public, reverse proxy ;
- second service ;
- UI operateur ;
- inference / model serving ;
- cloud GPU ;
- migration vers `container` ou refonte de sandbox ;
- usage de `db-layer` comme host de service.

## 4. Prerequis minimaux

### 4.1 Prerequis reels

- acces operatoire a `student` reussi ;
- identite node confirmee : `student` reste le node cible ;
- coherence Launchable <-> Registry toujours vraie ;
- le profil `lab` reste valide sur `student` ;
- le workspace et les sorties peuvent etre tenus sur `student` :
  - `/srv/openclaw/workspaces/ws-lab-student`
  - `/srv/openclaw/outputs/ws-lab-student/artifacts`
  - `/srv/openclaw/outputs/ws-lab-student/logs`
- ces chemins sont creatibles ou presents, et writables ;
- un runtime local de type `process` est disponible pour `lab` ;
- `python3` et `jupyter lab` ou equivalent sont disponibles dans l'environnement local prepare pour `base-python` ;
- le port `8888` est libre sur `127.0.0.1` ;
- un token de session unique peut etre genere sans reuse.

### 4.2 Champs / valeurs du Launchable a respecter tels quels

Les champs ci-dessous ne doivent pas etre reinterpretes ni elargis :

- `spec.profile: lab`
- `spec.node.target: student`
- `spec.workspace.root: /srv/openclaw/workspaces/ws-lab-student`
- `spec.bootstrap.profile: lab`
- `spec.bootstrap.preset: base-python`
- `spec.bootstrap.env.OPENCLAW_MODE: lab`
- `spec.bootstrap.env.WORKSPACE_KIND: interactive`
- `spec.policy.network.mode: loopback-only`
- `spec.policy.auth.mode: local-user`
- `spec.policy.sandbox.mode: process`
- `spec.policy.exposure.defaultDeny: true`
- `spec.services[0].name: notebook`
- `spec.services[0].type: jupyter`
- `spec.services[0].bind: 127.0.0.1`
- `spec.services[0].port: 8888`
- `spec.services[0].visibility: loopback`
- `spec.services[0].auth: token`
- `spec.outputs.artifactsPath: /srv/openclaw/outputs/ws-lab-student/artifacts`
- `spec.outputs.logsPath: /srv/openclaw/outputs/ws-lab-student/logs`

## 5. Sequence minimale d'ouverture locale

### Etape 1 - Verrouiller le cas exact

- charger `examples/launchable_lab_student.yaml` ;
- confirmer l'entree registry `launchable-lab-student-v1` ;
- confirmer que le verdict de classification reste `autorisable` ;
- arreter immediatement si une derive de policy apparait.

### Etape 2 - Verifier le terrain minimal sur `student`

- verifier acces a `student` ;
- verifier existence ou creation des chemins workspace / artifacts / logs ;
- verifier droits d'ecriture ;
- verifier disponibilite de `python3` et `jupyter lab` ;
- verifier que `127.0.0.1:8888` est libre.

### Etape 3 - Preparer la session locale

- se placer sous l'utilisateur local attendu sur `student` ;
- exporter les env du Launchable :
  - `OPENCLAW_MODE=lab`
  - `WORKSPACE_KIND=interactive`
- generer un token de session unique ;
- preparer un fichier PID et un fichier de log dans `logsPath` ;
- ne pas reutiliser un token precedent.

### Etape 4 - Ouvrir `jupyter` en loopback strict

Commande-type minimale acceptable :

```bash
mkdir -p \
  /srv/openclaw/workspaces/ws-lab-student \
  /srv/openclaw/outputs/ws-lab-student/artifacts \
  /srv/openclaw/outputs/ws-lab-student/logs

export OPENCLAW_MODE=lab
export WORKSPACE_KIND=interactive
export JUPYTER_TOKEN="$(python3 - <<'PY'
import secrets
print(secrets.token_urlsafe(24))
PY
)"

nohup jupyter lab \
  --no-browser \
  --ip=127.0.0.1 \
  --port=8888 \
  --ServerApp.token="$JUPYTER_TOKEN" \
  --ServerApp.allow_remote_access=False \
  --ServerApp.root_dir=/srv/openclaw/workspaces/ws-lab-student \
  > /srv/openclaw/outputs/ws-lab-student/logs/jupyter-lab.log 2>&1 &

echo $! > /srv/openclaw/outputs/ws-lab-student/logs/jupyter-lab.pid
```

Regles de cette etape :

- aucun bind autre que `127.0.0.1` ;
- aucun autre port ;
- aucun autre service ;
- aucune ouverture LAN ou publique ;
- aucun secret du control plane ;
- aucune execution sur `db-layer`.

### Etape 5 - Verifier l'ouverture locale

- verifier que le PID existe et appartient a l'utilisateur local ;
- verifier qu'un listener existe sur `127.0.0.1:8888` seulement ;
- verifier qu'une requete locale depuis `student` vers `http://127.0.0.1:8888` repond ;
- verifier que le log de demarrage existe dans `logsPath` ;
- ecrire une preuve minimale dans `artifactsPath`.

Exemple minimal de preuve d'ouverture :

```bash
printf '%s\n' \
  "launchable=lab-student" \
  "profile=lab" \
  "node=student" \
  "service=notebook" \
  "bind=127.0.0.1" \
  "port=8888" \
  "auth=token" \
  "status=opened" \
  > /srv/openclaw/outputs/ws-lab-student/artifacts/jupyter-open.txt
```

## 6. Preuves minimales attendues

Le minimum exigible pour considerer l'ouverture locale reussie est :

- preuve d'identite node : service ouvert sur `student` et pas ailleurs ;
- preuve de process : PID vivant sous l'utilisateur local ;
- preuve reseau : listener present sur `127.0.0.1:8888` seulement ;
- preuve auth : token de session non vide, non anonyme, non reutilise ;
- preuve HTTP locale : reponse observable depuis `student` vers `127.0.0.1:8888` ;
- preuve de log : `jupyter-lab.log` present dans `logsPath` ;
- preuve d'artefact : `jupyter-open.txt` ou equivalent present dans `artifactsPath` ;
- preuve de non-derive : aucun LAN, aucun public, aucun second service, aucun secours `db-layer`.

Regle de prudence :

- le token ne doit pas etre recopie en clair dans un rapport partage ;
- si le log contient une URL avec token, ce log reste une trace sensible locale.

## 7. Blockers / conditions d'arret

Les points suivants font echouer l'ouverture :

- Launchable ou registry incoherent ;
- profil `lab` non conforme ou node cible different de `student` ;
- chemins workspace / artifacts / logs non tenables ou non writables ;
- `python3` ou `jupyter lab` indisponible dans le contexte local ;
- port `127.0.0.1:8888` deja occupe ;
- service tente avec un bind autre que `127.0.0.1` ;
- token absent, vide, ou auth degradee ;
- listener non observable en loopback local ;
- la verification impose LAN, public, UI operateur, cloud, serving, ou fallback vers `db-layer` ;
- toute derive vers un deuxieme service ou un autre profil.

Regle d'arret : au premier blocker, on stoppe. On n'elargit pas le chantier pour "faire marcher" le service.

## 8. Rollback minimal

Si l'ouverture echoue ou doit etre annulee :

- arreter le process Jupyter via le PID enregistre ;
- verifier la disparition du listener `127.0.0.1:8888` ;
- supprimer les fichiers de session strictement temporaires crees pour l'ouverture, comme le PID et tout token ecrit localement en clair ;
- conserver ou assainir le log local selon son contenu sensible ;
- conserver l'artefact minimal seulement s'il reste utile et non sensible ;
- ne pas toucher au registry, a la policy KB-302, ni a KB-301 ;
- ne jamais reporter le service sur `db-layer`.

## 9. Hors perimetre explicite

Restent explicitement hors perimetre de ce micro-runbook :

- `private-lan` ;
- toute exposition Internet ou publique ;
- SSH reverse tunnel ou tunnel public persistant ;
- deuxieme service (`tensorboard`, `http-api`, autre notebook) ;
- dashboard ou UI operateur ;
- notebook multi-utilisateur ou plateforme partagee ;
- inference, serving, prediction API, vLLM ;
- cloud GPU ;
- tout service standard sur `db-layer`.

## 10. Definition of done

Cette mini-ouverture est `DONE` si et seulement si :

- le seul cas traite est `lab` + `jupyter` sur `student` ;
- les champs du Launchable sont respectes tels quels ;
- le service demarre en `process` local sous l'utilisateur attendu ;
- le bind reste strictement `127.0.0.1` sur le port `8888` ;
- l'auth par token est active ;
- une verification HTTP locale reussit depuis `student` ;
- un log minimal existe dans `logsPath` ;
- un artefact minimal existe dans `artifactsPath` ;
- aucun LAN, aucun public, aucun autre service, aucune derive vers `db-layer`, KB-303, KB-304 ou KB-305 n'a ete necessaire.
