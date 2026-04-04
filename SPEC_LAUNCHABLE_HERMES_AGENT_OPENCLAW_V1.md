# SPEC LAUNCHABLE HERMES AGENT OPENCLAW V1

## 1. But du document

Definir une spec documentaire pour un launchable Hermes Agent dans le cadre OpenClaw, sans inferer une activation runtime effective. Cette spec sert a decrire la forme attendue, les contraintes de classification et les limites de validite d un cas Hermes borne.

## 2. Perimetre

Cette spec decrit :
- un launchable Hermes Agent au format conceptuel OpenClaw ;
- les champs minimaux attendus ;
- les contraintes de role node ;
- les restrictions de securite et d exposition ;
- les conditions de classification documentaire.

Cette spec ne decrit pas :
- une installation Hermes reelle ;
- une image container construite ;
- une execution prouvee ;
- un deploiement reseau ;
- une validation de production.

## 3. Position doctrinale

Hermes Agent reste ici un objet de cadrage documentaire.

En consequence :
- la spec launchable Hermes n est pas une autorisation de deploiement ;
- la presence d un YAML Hermes ne prouve aucun runtime ;
- tout cas Hermes doit rester compatible avec les regles OpenClaw deja etablies.

## 4. Node cible autorisable

### 4.1 Node cible par defaut

Le seul node cible envisageable pour un cas Hermes borne est `student`.

### 4.2 Node explicitement non autorise par defaut

`db-layer` n est pas un node cible valide pour un usage Hermes standard.

Toute tentative Hermes sur `db-layer` serait :
- soit `INVALID` ;
- soit `VALID_WITH_RESTRICTION` uniquement dans un cas exceptionnel de validation locale, bornee, loopback, non intensive, et explicitement documente.

### 4.3 Node futur

`cloud-gpu-future` peut exister comme cible documentaire future mais ne doit pas etre lu comme une capacite active.

## 5. Forme minimale du launchable Hermes

Un launchable Hermes documentaire doit au minimum decrire :

- `id`
- `name`
- `purpose`
- `node_role`
- `execution_mode`
- `network_exposure`
- `auth_mode`
- `runtime_status`
- `security_posture`
- `notes`

## 6. Contraintes sur les champs

### 6.1 `id`

- identifiant stable ;
- lisible ;
- sans ambiguite sur la nature Hermes ;
- recommande : prefixe `hermes_`.

### 6.2 `name`

- nom humain lisible ;
- doit indiquer qu il s agit d un cas documentaire ou borne si le runtime n est pas etabli.

### 6.3 `purpose`

- doit decrire la fonction du cas Hermes ;
- exemples recevables : orchestration locale bornee, agent memoire locale, interface de test loopback.

### 6.4 `node_role`

- `student` pour un cas local borne ;
- `cloud-gpu-future` pour une cible documentaire future ;
- `db-layer` interdit par defaut pour un usage Hermes standard.

### 6.5 `execution_mode`

Valeurs possibles recommandees :
- `document_only`
- `local_bounded`
- `future`

Pour un premier cas Hermes, la valeur attendue est `document_only` ou `local_bounded` selon le niveau reel de preuve.

### 6.6 `network_exposure`

Valeurs recommandees :
- `none`
- `loopback_only`
- `future_unapproved`

Aucune exposition LAN ou publique ne doit etre implicite.

### 6.7 `auth_mode`

Le champ doit indiquer le principe d authentification sans stocker de secret.

Exemples recevables :
- `not_applicable`
- `token_externalized`
- `local_operator_controlled`

### 6.8 `runtime_status`

Valeurs recommandees :
- `documented_only`
- `bounded_probe_only`
- `future`
- `not_established`

Le statut doit rester conservateur. Pas de surclassement.

### 6.9 `security_posture`

Le champ doit rappeler les garde-fous minimaux :
- pas de secret dans le repo ;
- pas d exposition large par defaut ;
- pas de deploiement implicite sur `db-layer` ;
- pas de confusion entre cadrage et runtime.

## 7. Regles de classification

### 7.1 `VALID`

Un cas Hermes ne peut etre `VALID` que si :
- son statut documentaire est coherent ;
- sa cible node respecte la doctrine ;
- son niveau d exposition est borne ;
- aucune promesse runtime non prouvee n apparait.

### 7.2 `VALID_WITH_RESTRICTION`

Un cas Hermes peut etre `VALID_WITH_RESTRICTION` si :
- la cible ou l usage requiert une reserve explicite ;
- le runtime n est pas etabli mais le cadrage est coherent ;
- l exposition doit rester `none` ou `loopback_only` ;
- une preuve future est necessaire avant tout usage plus large.

### 7.3 `INVALID`

Un cas Hermes est `INVALID` si :
- il traite `db-layer` comme node de compute standard ;
- il suppose une exposition LAN/public sans policy dediee ;
- il embarque secrets, credentials, configs reelles ;
- il transforme le repo OpenClaw en repo runtime.

## 8. Interdits explicites

- pas de secret dans le YAML ;
- pas de chemin machine-specifique sensible ;
- pas de commande shell reelle ;
- pas d image ou de hash runtime non gouverne ;
- pas d URL publique implicite ;
- pas de promesse de service actif non prouve.

## 9. Exemple de lecture attendue

Un premier launchable Hermes dans OpenClaw doit etre lu comme :

- une fiche de cadrage executable seulement au sens documentaire ;
- une base pour future qualification ;
- une specification de forme ;
- pas une preuve d integration effective.

## 10. Condition de mise a jour

Mettre a jour cette spec seulement si :
- un schema canonique OpenClaw evolue ;
- un cas Hermes borne est explicitement ouvert ;
- une policy de classification Hermes est ajoutee ;
- les frontieres node/exposition changent.

Ne pas la mettre a jour pour ajouter :
- notes brutes ;
- essais runtime locaux non qualifies ;
- valeurs sensibles ;
- instructions d installation machine-specifiques.

## 11. Point de reprise

Point de reprise naturel apres cette spec :
- produire un exemple YAML documentaire Hermes borne ;
- ou ouvrir un rapport de classification Hermes ;
- ou s arreter au niveau spec si aucun GO runtime borne n existe.
