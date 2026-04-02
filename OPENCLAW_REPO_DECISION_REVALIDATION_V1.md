# OPENCLAW REPO DECISION REVALIDATION V1

## 1. Etat de depart repris

- Decision precedente : `repo_dedie_pas_encore_recommande`.
- Motif principal precedent : manque de socle documentaire de gouvernance et d ouverture operatoire pour un nouvel operateur.
- Etat maintenant acquis : `WORKFLOW_OPENCLAW.md`, `REPO_BOUNDARIES_OPENCLAW.md`, `ETABLI_OPENCLAW.md` et `SESSION_OPENING_INDEX.md` existent et sont relisibles.
- Socle OpenClaw confirme : spec, registry, validation, bootstrap, smoke, result contract, mapping infra, policy securite, KB-301 `DONE`, KB-302 policy `DONE`.
- Perimetre de la presente revalidation : gouvernance documentaire uniquement. Aucun repo GitHub n est cree ici.

## 2. Evolution depuis la decision precedente

- Le manque principal precedent est reellement reduit : le socle de gouvernance minimal demande existe maintenant.
- `WORKFLOW_OPENCLAW.md` fixe les regles de travail, les priorites documentaires, les interdits et les conditions de maturite avant repo dedie.
- `REPO_BOUNDARIES_OPENCLAW.md` borne proprement un futur repo doc/gouvernance-only et exclut runtime, secrets, logs et configurations machine.
- `ETABLI_OPENCLAW.md` donne un etat acquis lisible, avec limites explicites et sans surestimer la maturite runtime.
- `SESSION_OPENING_INDEX.md` donne enfin un point d entree minimal pour un nouvel operateur sans memoire externe diffuse.
- Le perimetre du futur repo reste donc beaucoup plus clair et plus sain qu au moment de la premiere decision.

## 3. Conditions maintenant remplies

Les conditions suivantes sont maintenant remplies de facon exploitable :

- documentation de gouvernance minimale en place ;
- frontieres repo / hors repo explicites ;
- separation control plane / compute plane explicite ;
- etat acquis reel vs futur explicite ;
- index minimal d ouverture de session disponible ;
- perimetre initial doc/gouvernance-only defendable ;
- absence de besoin de melanger OpenClaw avec opt-trading pour comprendre le socle ;
- matiere canonique suffisante pour peupler un repo documentaire borne.

## 4. Manques restants reels

Deux manques reels subsistent encore au regard du workflow canonique actuel :

- `KANBAN_OPENCLAW.md` n existe pas sous ce nom canonique, alors que `WORKFLOW_OPENCLAW.md` le cite dans la condition de maturite minimale avant repo dedie et que `REPO_BOUNDARIES_OPENCLAW.md` l inclut dans les documents de gouvernance du futur repo ;
- `README.md` d entree du futur repo n existe pas au niveau canonique attendu, alors que `WORKFLOW_OPENCLAW.md` le demande explicitement comme point d entree projet.

Lecture stricte :

- `OPENCLAW_PLAN_KANBAN_CANONIQUE_V1.md` joue partiellement le role de kanban, mais ne remplit pas formellement la condition nommee `KANBAN_OPENCLAW.md` telle qu ecrite dans le workflow ;
- `SESSION_OPENING_INDEX.md` couvre l ouverture de session, mais ne remplace pas formellement un `README.md` de repo.

Ces manques ne justifient pas un chantier runtime, mais ils empechent encore une recommandation nette si l on applique strictement la condition de maturite definie par le workflow canonique en vigueur.

## 5. Verdict final

`repo_dedie_pas_encore_recommande`

Motif net :

- le socle documentaire est maintenant presque suffisant et sain ;
- le manque principal precedent de gouvernance minimale est traite ;
- mais la condition de maturite minimale ecrite dans `WORKFLOW_OPENCLAW.md` n est pas encore entierement satisfaite faute de `KANBAN_OPENCLAW.md` et de `README.md` canoniques.

En consequence, la recommandation ne bascule pas encore.

## 6. Conditions avant creation effective du repo

Avant creation effective du repo GitHub dedie, il faut seulement fermer les deux ecarts documentaires restants deja justifies par le canon existant :

- produire `KANBAN_OPENCLAW.md` comme vue d etat canonique des chantiers ;
- produire `README.md` comme point d entree rapide du futur repo.

Conditions deja acquises a conserver sans les rouvrir :

- `WORKFLOW_OPENCLAW.md` ;
- `REPO_BOUNDARIES_OPENCLAW.md` ;
- `ETABLI_OPENCLAW.md` ;
- `SESSION_OPENING_INDEX.md` ;
- repo doc/gouvernance-only au demarrage ;
- aucun secret, aucun runtime effectif, aucune migration code depuis opt-trading.

## 7. Plus petit prochain pas utile

- Produire `KANBAN_OPENCLAW.md` et `README.md` au niveau canonique minimal.
- Puis relancer une revalidation documentaire finale courte.
- Ne pas creer le repo GitHub avant cette cloture.
