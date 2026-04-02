# SECURITY POLICY OPENCLAW BREV V1

## 1. Objet

Cette policy fixe les garde-fous minimaux de securite pour la couche OpenClaw Brev-like V1. Elle s'applique aux launchables, aux services declares et aux futures extensions de compute.

## 2. Trust boundaries

Les frontieres de confiance de la V1 sont les suivantes :
- poste operateur : origine des actions d'administration, hors execution cible ;
- `db-layer` : control plane, policy plane, stockage des definitions et coordination ;
- `student` : compute local, separe du control plane meme s'il est dans le meme environnement ;
- `cloud-gpu-future` : compute externe, frontiere de confiance distincte et plus restrictive.

Regle directrice : une frontiere de confiance ne doit pas recevoir implicitement les droits, secrets ou ouvertures d'une autre.

## 3. Auth

### Principes
- aucune exposition de service actif sans mecanisme d'auth explicite ;
- pas de compte partage implicite ;
- pas de token statique reutilise entre launchables ;
- les identites operatoires doivent rester tracables.

### Modes admis en V1
- `local-user` pour les usages strictement locaux et attaches a l'operateur ou a un compte technique local ;
- `token` pour les services applicatifs tels que notebook, API locale ou TensorBoard ;
- `ssh-key` pour les cas controles qui en ont besoin.

### Interdictions
- `auth: none` sur un service actif ;
- reuse d'un meme jeton pour plusieurs launchables de nature differente ;
- ouverture de service avec credentials par defaut non changes.

## 4. Loopback et exposition reseau

### Regle par defaut
- tout service actif est lie a `127.0.0.1` par defaut ;
- `spec.policy.exposure.defaultDeny` doit rester a `true` ;
- aucune exposition publique ou LAN n'est implicite.

### Exposition autorisable, mais non par defaut
Un service ne peut sortir de la boucle locale que si les conditions suivantes sont toutes vraies :
- besoin operatoire explicite documente ;
- mode reseau compatible dans la policy ;
- auth activee ;
- journalisation prevue ;
- revue explicite avant ouverture.

### Ce qu'il est interdit d'ouvrir par defaut
- `0.0.0.0` sur notebook, API ou TensorBoard ;
- ports publics entrants sur `db-layer` ;
- tunnels persistants non documentes ;
- acces anonyme a un service applicatif ;
- exposition directe d'un port de compute cloud futur vers Internet.

## 5. Sandboxing

### Regle minimale
Chaque launchable doit declarer un mode de sandbox parmi `process`, `container`, `restricted-container`.

### Doctrine
- `process` est reserve aux usages locaux simples et maitrises ;
- `container` est prefere quand l'isolation d'environnement est utile ;
- `restricted-container` est prefere pour `inference` locale sur `db-layer` et pour les cibles externes futures.

### Interdictions minimales
- pas de montage implicite de secrets du control plane dans un launchable de compute ;
- pas de partage du socket Docker ou d'un acces root non borne par defaut ;
- pas de privilege eleve sans justification documentaire separee.

## 6. Policy des services exposables

En V1, seuls les types suivants sont reconnus dans la spec :
- `jupyter`
- `http-api`
- `tensorboard`

Pour chacun :
- activation explicite service par service ;
- port explicite ;
- auth explicite ;
- visibilite explicite ;
- bind explicite.

Un service non decrit dans la spec n'est pas autorise dans un exemple canonique V1.

## 7. Regles minimales avant cloud GPU

Avant toute activation reelle de `cloud-gpu-future`, il faut au minimum :
- un inventaire des acces SSH et des comptes techniques ;
- une separation claire des secrets entre control plane et compute externe ;
- une policy d'egress minimale et des destinations connues ;
- un mode de sandbox cible ;
- l'interdiction de tout port public expose par defaut ;
- une validation documentaire du type de services autorises.

Sans ces points, `cloud-gpu-future` reste un target declaratif seulement.

## 8. Cas particulier de `db-layer`

`db-layer` heberge le control plane. En consequence :
- les launchables sur `db-layer` doivent rester exceptionnels et bornes ;
- seul le profil `inference` local de validation est tolere en V1 ;
- tout service sur `db-layer` doit rester en loopback sauf decision separee ;
- aucun usage GPU lourd ou multitennant n'est admis sur cette machine dans la presente passe.

## 9. Resume operatoire

- default deny partout ;
- loopback par defaut ;
- auth obligatoire pour tout service actif ;
- separation stricte control plane / compute plane ;
- ouverture cloud reportee tant que les preconditions minimales ne sont pas reunies.
