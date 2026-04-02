# KB302 EXPOSABLE SERVICES POLICY V1

## 1. But de KB-302

KB-302 existe pour cadrer les services exposables sur `student` apres KB-301, pas pour les deployer largement.

Le chantier s'ouvre seulement apres KB-301 car il fallait d'abord qualifier un vrai node compute local. Une policy de services sans node reel integre aurait ete theorique et fragile.

Question canonique de KB-302 :

> quels services peuvent etre exposes depuis `student`, sous quelles conditions minimales, avec quels garde-fous, et avec quels interdits, sans ouvrir KB-303, KB-304 ou KB-305 ?

## 2. Socle canonique rappele

- la phase 2 documentaire socle est `CLOSE` ;
- KB-301 est `DONE` ;
- `student` est le premier compute node local reel integre ;
- `db-layer` reste le control plane OpenClaw ;
- `db-layer` n'est pas un compute plane generaliste ;
- seule exception sur `db-layer` : `inference` locale de validation, bornee, loopback, non intensive, exceptionnelle ;
- KB-302 ne reouvre ni la phase 2, ni KB-301.

Consequence operative :

- les services standards raisonnes ici visent `student` ;
- aucun service standard ne doit etre "reporte" sur `db-layer` ;
- le chantier reste un cadrage de policy, pas un plan de deploiement large.

## 3. Definition d'un service exposable

Un service exposable, dans KB-302, est :

- une surface active declaree dans `spec.services[]` d'un Launchable cible `student` ;
- accessible via un `bind`, un `port`, une `visibility` et une `auth` explicites ;
- destinee a un usage operatoire borne attache a un launchable ;
- soumise a une policy reseau, auth, sandbox et journalisation explicites.

Un service exposable n'est donc pas :

- un service public par defaut ;
- un produit multi-tenant ;
- une UI operateur globale ;
- un endpoint de serving modele ;
- un pretexte pour requalifier `db-layer`.

Dans le canon V1 actuel, les seuls types reconnus par la spec sont :

- `jupyter`
- `http-api`
- `tensorboard`

Tout autre type reste hors policy KB-302 tant qu'il n'est pas ajoute explicitement a la spec.

## 4. Classes de services sur `student`

### 4.1 Classes d'exposition

| Classe | Lecture | Statut KB-302 |
|---|---|---|
| `local-loopback-only` | service borne a `127.0.0.1`, attache a l'operateur local | classe standard par defaut |
| `private-lan-controlled` | service visible sur un LAN prive controle, jamais implicite | classe exceptionnelle, sous conditions |
| `future-not-opened` | surface imaginable mais dependante d'un chantier suivant ou d'une evolution de spec | non ouverte maintenant |
| `forbidden-in-kb302` | surface qui viole la doctrine, la security policy ou la frontiere de chantier | interdite |

### 4.2 Types de services et statut canonique

| Type / usage | Statut | Lecture operatoire |
|---|---|---|
| `jupyter` loopback sur `lab` | `autorisable` | cas canonique naturel sur `student`, si prerequis complets |
| `jupyter` loopback sur `vision` | `autorisable_sous_conditions` | seulement si le profil `vision` lui-meme redevient qualifiable sur `student` |
| `jupyter` en `private-lan` | `autorisable_sous_conditions` | jamais par defaut, besoin explicite et revue obligatoire |
| `tensorboard` loopback | `autorisable_sous_conditions` | acceptable seulement comme surface technique bornee, si le profil support existe reellement |
| `tensorboard` en `private-lan` | `autorisable_sous_conditions` | meme garde-fous que LAN controle, jamais public |
| `http-api` locale de travail, non serving, loopback | `autorisable_sous_conditions` | seulement pour une API technique de session, pas pour du serving modele |
| `http-api` en `private-lan` | `non_autorisable` dans KB-302 | trop proche d'un endpoint partage ou de serving ; bascule vers KB-305 si besoin reel |
| tout service actif sur `hf_build` | `non_autorisable` | le profil `hf_build` reste un profil compute sans surface de service dans le canon actuel |
| type non reconnu par la spec | `non_autorisable` | hors spec V1 |
| notebook hub, dashboard operateur, front multi-launchables | `non_autorisable` ici | derive vers KB-303 |
| endpoint inference, vLLM, API de prediction, serving GPU | `non_autorisable` ici | derive vers KB-305 |
| exposition Internet / publique | `non_autorisable` | hors perimetre KB-302 |

## 5. Policy reseau

### 5.1 Regle de base

- `default deny` partout ;
- `loopback` par defaut ;
- aucun service actif n'est implicitement autorise en LAN ou Internet ;
- `0.0.0.0` n'est pas un bind acceptable par defaut ;
- aucun tunnel public, reverse proxy public, DNS public ou publication Internet n'entre dans KB-302.

### 5.2 Classe standard : `local-loopback-only`

Un service `local-loopback-only` est la forme normale d'un service exposable sur `student`.

Conditions minimales :

- `visibility: loopback` ;
- `bind: 127.0.0.1` ;
- pas d'entree publique ni LAN ;
- usage borne a l'operateur ou a la session de travail locale.

### 5.3 Classe exceptionnelle : `private-lan-controlled`

Une ouverture LAN controlee est possible en doctrine, mais seulement comme exception gouvernee sur `student`.

Conditions minimales cumulatives :

- besoin operatoire explicite documente ;
- service deja recevable en mode loopback sur le fond ;
- `network.mode: private-lan` ;
- `visibility: private-lan` ;
- bind explicite sur une adresse privee controlee, jamais `0.0.0.0` ;
- auth forte active ;
- journalisation prevue ;
- revue explicite avant ouverture ;
- aucune lecture possible comme service public, produit partage ou endpoint de plateforme.

### 5.4 Internet / public

Dans KB-302, l'exposition Internet ou publique est interdite.

Sont explicitement hors policy autorisable :

- publication Internet directe ;
- tunnels publics persistants ;
- ports publics entrants ;
- LB, DNS, TLS publics ;
- partage anonyme ou non borne d'un notebook, d'une API ou de TensorBoard.

## 6. Policy auth / sandbox / isolation

### 6.1 Auth minimale obligatoire

- aucun service actif sans auth explicite ;
- `auth: none` sur un service actif = `non_autorisable` ;
- pas de token statique partage entre launchables differents ;
- pas de credentials par defaut non changes ;
- l'identite operatoire doit rester tracable.

### 6.2 Auth par classe

- `loopback` : `token` prefere ; `local-user` seulement pour un usage strictement local et non partage ;
- `private-lan-controlled` : `token` ou `ssh-key` obligatoires ; `local-user` seul est insuffisant ;
- `http-api` autorisable en KB-302 : `token` obligatoire.

### 6.3 Sandbox minimale

- `process` n'est acceptable que pour un service local simple, loopback-only, faible risque, attache a un seul operateur ;
- `container` est la base recommandee pour un service exposable sur `student` ;
- `http-api` n'est jamais autorisable en `process` dans KB-302 ;
- toute ouverture `private-lan-controlled` exige au minimum `container`.

### 6.4 Isolation minimale

- pas de montage implicite de secrets du control plane ;
- pas de partage du socket Docker ;
- pas de privilege root ou privilegie sans justification separee ;
- pas de confusion entre workspace de compute et stockage canonique de policy ;
- `student` n'heberge pas la source de verite du control plane.

## 7. Conditions d'autorisabilite

Un service sur `student` n'est `autorisable` ou `autorisable_sous_conditions` que si toutes les conditions ci-dessous sont vraies.

### 7.1 Prerequis documentaires

- le Launchable est valide selon KB-202 ;
- le Launchable correspond a une entree de registry ;
- `phase-2-early` est autorise ;
- le couple `profile/node` reste coherent ;
- le service utilise un type reconnu par la spec.

### 7.2 Prerequis de node et de profil

- le node cible est `student` ;
- `student` reste qualifie par KB-301 ;
- le profil support du service est lui-meme qualifiable sur `student` ;
- le service reste naturel pour le profil vise.

Lecture pratique actuelle :

- `lab` peut porter un `jupyter` loopback canonique ;
- `vision` ne peut porter un service qu'une fois son propre prerequis compute retabli ;
- `hf_build` ne doit pas devenir un profil de service ;
- `training` sur `student` n'est pas traite ici tant qu'un launchable correspondant n'est pas canonise.

### 7.3 Prerequis reseau / auth / sandbox

- `defaultDeny: true` ;
- bind, port, visibility et auth explicites ;
- classe reseau compatible avec la policy ;
- auth adequate a la classe d'exposition ;
- sandbox adequate au risque du service ;
- aucune dependance a une ouverture publique.

### 7.4 Prerequis operatoires minimaux

- workspace, logs et artefacts tenables sur `student` ;
- journalisation locale minimale prevue ;
- raison d'usage courte et comprehensible ;
- duree et perimetre bornes ;
- aucune dependance a KB-303, KB-304 ou KB-305 pour justifier l'ouverture.

## 8. Blockers et interdits

Les cas suivants conduisent a `non_autorisable`.

- service actif sans auth explicite ;
- `auth: none` sur un service actif ;
- bind `0.0.0.0` ou exposition publique ;
- tunnel public ou publication Internet ;
- type de service hors spec V1 ;
- Launchable invalide ou incoherent avec le registry ;
- profil sous-jacent non qualifiable sur `student` ;
- service qui suppose un cloud GPU, une UI operateur, ou un serving modele ;
- service qui transforme `hf_build` en profil de service ;
- `http-api` qui sert un endpoint partage, durable ou de prediction ;
- service qui requiert secrets du control plane, privilege eleve ou socket Docker ;
- toute tentative de deplacement du besoin vers `db-layer`.

## 9. Protection doctrinale de `db-layer`

Section non negociable.

- KB-302 ne cree aucune classe de service standard sur `db-layer`.
- `db-layer` ne devient ni notebook host, ni API host standard, ni TensorBoard host.
- le seul cas admis sur `db-layer` reste l'exception existante de validation locale `inference`, bornee, loopback, non intensive, exceptionnelle ;
- cette exception n'est pas une base d'autorisation de services standards ;
- aucun echec ou inconfort sur `student` n'autorise un glissement pratique vers `db-layer` ;
- si un service n'est pas autorisable sur `student`, le verdict reste negatif ; il n'est pas "sauve" par `db-layer`.

## 10. Frontieres exactes du chantier

### 10.1 Ce que KB-302 couvre

- definition d'un service exposable ;
- classes de services reconnues ;
- policy reseau, auth, sandbox et isolation ;
- conditions d'autorisabilite ;
- blockers et interdits ;
- protection explicite de `db-layer` ;
- frontiere avec les chantiers suivants.

### 10.2 Ce que KB-302 ne couvre pas

- implementation reelle d'un service ;
- deploiement, tokenisation, runbooks operatoires d'ouverture ;
- dashboards, frontends operateur, lecture multi-launchables ;
- integration cloud GPU ;
- inference runtime, model serving, endpoints de prediction ;
- exposition publique ou tunnels persistants ;
- orchestration complete ou multi-node.

### 10.3 Frontiere avec KB-303

Releve de KB-303 :

- dashboard operateur ;
- UI de supervision ;
- front multi-launchables ;
- lecture d'etat centralisee.

### 10.4 Frontiere avec KB-304

Releve de KB-304 :

- ouverture de services sur `cloud-gpu-future` ;
- comptes techniques cloud, SSH, reseaux externes, secrets cloud ;
- ingress externe et trust boundary cloud.

### 10.5 Frontiere avec KB-305

Releve de KB-305 :

- inference exposee ;
- endpoint de prediction ;
- vLLM et serving modele ;
- API durable de modele ;
- surface reseau d'inference GPU.

## 11. Verdicts autorises

### `autorisable`

Service reconnu, borne a `student`, compatible avec un profil lui-meme qualifiable, en loopback par defaut, avec auth explicite, sandbox adequate, et sans derive vers un chantier suivant.

### `autorisable_sous_conditions`

Service recevable en doctrine, mais seulement avec conditions supplementaires explicites :

- prerequis de profil encore a retablir ;
- ou revue explicite pour `private-lan-controlled` ;
- ou garde-fous renforces pour une surface technique comme `tensorboard` ou `http-api` locale de travail.

### `non_autorisable`

Service qui viole la spec, la security policy, la doctrine infra, le role de `db-layer`, ou qui ouvre de fait KB-303, KB-304 ou KB-305.

## 12. Definition of done KB-302

KB-302 est `DONE` si et seulement si :

- un service exposable est defini sans ambiguite ;
- les types de services reconnus sur `student` sont classes ;
- la policy reseau minimale est explicite ;
- les exigences auth / sandbox / isolation sont explicites ;
- les prerequis d'autorisabilite sont ecrits ;
- les blockers et interdits sont ecrits ;
- la protection doctrinale de `db-layer` est explicite ;
- la frontiere avec KB-303, KB-304 et KB-305 est nette ;
- le document permet de classer toute proposition de service sur `student` en `autorisable`, `autorisable_sous_conditions` ou `non_autorisable` sans rouvrir l'architecture.
