# Protection des données NeoMundi

Architecture de protection des données, minimisation des données et modes de traitement de l’instrument de mesure runtime NeoMundi.

NeoMundi est conçu pour mesurer les systèmes d’IA générative en temps réel tout en minimisant l’exposition aux contenus des clients.

Ce dépôt documente l’architecture de protection des données de l’instrument NeoMundi, notamment :

- Mode OBS : observabilité post-génération sans transmission de contenu sémantique.
- Mode GOV : gouvernance runtime avec traitement en flux transitoire.
- Architecture BYOK : le client conserve le contrôle de sa clé fournisseur LLM.
- Aucun stockage des contenus : les prompts, réponses et contenus générés ne sont pas conservés par NeoMundi.
- Artefacts de mesure minimaux : NeoMundi produit des signaux techniques, pas des bases de données de contenus.
- Séparation des responsabilités entre contenu client, mesure runtime et décisions de gouvernance.

Ce dépôt vise à faciliter la revue technique, l’évaluation sécurité, la revue juridique et la préparation d’un DPA.

Il ne constitue pas un contrat juridique.

---

## 1. Objet de ce dépôt

L’objet de ce dépôt est de fournir une description claire, vérifiable et versionnée de l’architecture de protection des données de NeoMundi.

NeoMundi fournit un instrument de mesure runtime pour les systèmes d’IA générative.

L’instrument produit des signaux utiles à la gouvernance, notamment :

- stabilité runtime ;
- dérive ;
- signaux de validité ;
- risque d’hallucination ;
- cohérence sémantique ;
- densité informationnelle ;
- statut de gouvernance ;
- artefacts de mesure.

NeoMundi ne cherche pas à posséder, stocker ou exploiter les contenus des clients.

Le principe central est :

> Mesurer le comportement des systèmes d’IA sans conserver leurs contenus.

---

## 2. Principe central de protection des données

NeoMundi repose sur une séparation stricte entre trois niveaux :

1. Contenu applicatif du client  
   Les prompts, réponses, données utilisateurs, données métier, documents, instructions et sorties générées restent sous la responsabilité et le contrôle du client.

2. Traitement de mesure runtime  
   NeoMundi peut traiter des signaux techniques ou sémantiques selon le mode sélectionné, uniquement dans le but de produire des signaux de mesure.

3. Artefacts de mesure  
   NeoMundi produit des artefacts minimaux tels que des identifiants de requête, horodatages, mesures de stabilité, signaux de dérive et statuts de gouvernance.

Cette séparation est conçue pour soutenir :

- la minimisation des données ;
- la limitation de finalité ;
- le privacy by design ;
- le privacy by default ;
- l’auditabilité ;
- la gouvernance contrôlée par le client.

---

## 3. Modes opérationnels

NeoMundi distingue deux modes opérationnels :

- OBS : mode Observabilité.
- GOV : mode Gouvernance.

Ces modes diffèrent selon le moment où l’instrument est appelé, les données transmises et la nature du traitement effectué.

---

## 4. Mode OBS : observabilité après génération

Le mode OBS est conçu pour l’observabilité post-génération.

En mode OBS, NeoMundi n’est pas placé dans le flux de génération en direct.

Le système client génère la réponse avec sa propre pile LLM, son infrastructure et sa configuration fournisseur.

NeoMundi reçoit uniquement les métriques, traces techniques ou artefacts d’observation nécessaires à la production des signaux de mesure.

### Propriétés clés du mode OBS

En mode OBS :

- NeoMundi intervient après la génération.
- Le contenu sémantique n’est pas transmis à NeoMundi.
- Les prompts ne sont pas transmis.
- Les réponses ne sont pas transmises.
- Les contenus générés ne sont pas transmis.
- Le client conserve le plein contrôle de sa clé fournisseur LLM.
- NeoMundi produit des signaux d’observabilité à partir des artefacts fournis.
- Le client conserve l’autorité de décision.

Le mode OBS est adapté à :

- la surveillance post-génération ;
- l’analyse statistique ;
- la comparaison de modèles ;
- le benchmarking ;
- l’observabilité interne ;
- les protocoles de recherche ;
- les workflows de mesure sans enforcement.

### Résumé du mode OBS

    Le client génère le contenu
            ↓
    Le client extrait ou fournit des artefacts d’observation
            ↓
    NeoMundi mesure
            ↓
    NeoMundi renvoie des signaux
            ↓
    Le client interprète et gouverne

---

## 5. Mode GOV : gouvernance runtime pendant la génération

Le mode GOV est conçu pour la gouvernance runtime.

En mode GOV, NeoMundi est placé dans le flux de génération afin de produire des signaux de mesure pendant l’exécution.

Dans ce mode, le contenu sémantique transite par NeoMundi pendant la génération.

Ce traitement est strictement transitoire et limité à la mesure runtime.

NeoMundi ne conserve pas les prompts, réponses ou contenus générés.

### Propriétés clés du mode GOV

En mode GOV :

- NeoMundi intervient pendant la génération.
- Le contenu sémantique transite par NeoMundi pour la mesure runtime.
- Le traitement est effectué sous forme de traitement en flux transitoire.
- Les prompts ne sont pas conservés.
- Les réponses ne sont pas conservées.
- Les contenus générés ne sont pas conservés.
- Le contenu n’est pas indexé.
- Le contenu n’est pas réutilisé.
- Le contenu n’est pas utilisé pour l’entraînement de modèles.
- Le client conserve le contrôle de sa clé fournisseur LLM via l’architecture BYOK.
- NeoMundi produit des signaux de gouvernance runtime.
- Le client, le système client, la politique configurée ou l’opérateur responsable conserve l’autorité de décision.

### Résumé du mode GOV

    Le client appelle NeoMundi
            ↓
    NeoMundi traite le flux de génération de manière transitoire
            ↓
    NeoMundi mesure la stabilité, la dérive et les signaux de risque
            ↓
    NeoMundi renvoie des artefacts de gouvernance
            ↓
    Le système client applique sa propre politique

---

## 6. Traitement en flux transitoire

En mode GOV, le contenu sémantique est traité en flux pendant la génération.

Cela signifie que NeoMundi peut analyser le flux de génération dans l’unique but de produire des signaux de mesure runtime.

Ce traitement est :

- temporaire ;
- limité à une finalité déterminée ;
- non persistant ;
- non utilisé pour l’entraînement ;
- non utilisé pour construire une base de données de contenus ;
- non réutilisé pour du profilage commercial ;
- non conservé après l’opération de mesure.

Le traitement en flux transitoire a pour finalité de mesurer le comportement du système d’IA pendant sa génération.

Le transit ne signifie pas le stockage.

---

## 7. Architecture BYOK

NeoMundi suit une architecture BYOK.

BYOK signifie : Bring Your Own Key.

Le client utilise sa propre clé fournisseur LLM.

NeoMundi ne fournit pas la clé d’inférence LLM sous-jacente du client.

Le client reste responsable de :

- sa relation avec le fournisseur LLM ;
- son choix de modèle ;
- sa configuration fournisseur ;
- les conditions de traitement des données côté fournisseur ;
- ses propres données utilisateurs ;
- son propre contenu applicatif ;
- ses propres politiques de gouvernance.

NeoMundi agit comme une couche de mesure runtime au-dessus de la pile IA du client.

---

## 7.1 Composants de mesure tiers

NeoMundi peut utiliser des services techniques tiers pour produire certains signaux de mesure.

La génération LLM principale du client reste sous le contrôle du client via l’architecture BYOK.

Cependant, certains composants de mesure NeoMundi peuvent s’appuyer sur des fournisseurs externes pour des tâches spécifiques de scoring ou d’évaluation.

Au stade actuel, NeoMundi peut utiliser un modèle OpenAI comme LLM judge interne pour certains signaux de mesure, tels que le risque d’hallucination, la validité sémantique ou l’évaluation de cohérence, selon la configuration.

Cet usage est limité à des finalités de mesure.

NeoMundi n’utilise pas les composants de mesure tiers pour stocker le contenu client, constituer des datasets de contenus, entraîner des modèles sur le contenu client ou produire des analyses sans rapport avec la mesure.

Lorsqu’un composant de mesure tiers reçoit du contenu sémantique, cette transmission est limitée à la finalité de production du signal de mesure configuré.

Ce dépôt doit donc être lu conjointement avec les conditions applicables, les addenda de traitement des données et la documentation sécurité du fournisseur tiers concerné.

Les versions futures de NeoMundi pourront prendre en charge des fournisseurs de judge alternatifs ou configurables, notamment auto-hébergés, hébergés dans l’Union européenne ou sélectionnés par le client.

L’objectif de cette section est la transparence : l’architecture privacy de NeoMundi doit documenter non seulement ce que NeoMundi stocke, mais aussi les composants techniques susceptibles de participer à la production des signaux de mesure.

---

## 8. Aucun stockage des contenus

NeoMundi ne conserve pas les contenus clients.

Dans les modes OBS et GOV, NeoMundi ne stocke pas :

- les prompts ;
- les réponses ;
- les sorties générées ;
- les documents téléversés ;
- les conversations ;
- les messages utilisateurs ;
- les contenus métier ;
- les bases de données de contenus sémantiques.

NeoMundi ne réutilise pas les contenus clients pour :

- l’entraînement de modèles ;
- le fine-tuning ;
- l’enrichissement commercial ;
- l’indexation de contenus ;
- la création de datasets ;
- des analyses sans rapport avec la finalité de mesure.

La finalité de NeoMundi est de produire des signaux de mesure et des artefacts de gouvernance, pas d’accumuler du contenu client.

---

## 9. Artefacts de mesure minimaux

NeoMundi produit des artefacts minimaux conçus pour l’observabilité, la traçabilité et la gouvernance.

Selon la configuration, les artefacts peuvent inclure :

- identifiant de requête ;
- horodatage ;
- score de stabilité ;
- signal de dérive ;
- statut de gouvernance ;
- signal de risque ;
- signal de validité ;
- indicateur de risque d’hallucination ;
- indicateur de cohérence ;
- métrique de densité informationnelle ;
- classification de régime ;
- statut de décision tel que ALLOW, FLAG ou OBSERVE ;
- métadonnées techniques nécessaires à la traçabilité.

Ces artefacts ne sont pas destinés à reconstruire le contenu original.

Le client reste responsable de toute corrélation entre :

- identifiants internes de requête ;
- utilisateurs finaux ;
- contenus générés ;
- journaux applicatifs ;
- décisions de gouvernance ;
- contexte métier.

---

## 10. Autorité de décision

NeoMundi fournit des signaux.

NeoMundi ne remplace pas l’autorité de décision du client.

Le système client, la politique configurée ou l’opérateur responsable demeure responsable de :

- interpréter les signaux ;
- configurer les seuils ;
- décider quand observer ;
- décider quand alerter ;
- décider quand ralentir ;
- décider quand bloquer ;
- décider quand escalader vers une revue humaine ;
- documenter sa propre politique de gouvernance.

La position de NeoMundi est :

> Nous fournissons le signal.  
> Vous conservez l’autorité de décision.

---

## 11. Séparation entre signal et contenu

L’architecture NeoMundi sépare le contenu du signal.

Le contenu appartient au client.

Le signal est produit par l’instrument de mesure.

Cette séparation est centrale dans l’architecture NeoMundi.

Elle permet aux équipes clientes d’utiliser NeoMundi pour :

- l’observabilité IA ;
- la gouvernance runtime ;
- l’audit interne ;
- la préparation conformité ;
- la surveillance des risques ;
- la comparaison de modèles ;
- la supervision d’agents ;
- la surveillance de chaînes d’orchestration ;
- l’évaluation scientifique.

Sans exiger que NeoMundi conserve le contenu sous-jacent.

---

## 12. Privacy by design

NeoMundi est conçu selon des principes de privacy by design.

Cela signifie que la protection des données est prise en compte au niveau de l’architecture, et non ajoutée après coup.

L’architecture repose sur :

- la séparation des rôles ;
- des finalités de traitement limitées ;
- l’absence de conservation des contenus ;
- des artefacts minimaux ;
- un traitement runtime transitoire lorsque nécessaire ;
- le contrôle côté client de la corrélation ;
- une clé fournisseur LLM contrôlée par le client ;
- une gouvernance configurable ;
- des modes opérationnels documentés.

---

## 13. Privacy by default

NeoMundi suit également une approche privacy by default.

Par défaut, le système est conçu pour éviter toute conservation inutile des contenus.

Les hypothèses de conception par défaut sont :

- ne pas stocker les prompts ;
- ne pas stocker les réponses ;
- ne pas stocker les contenus générés ;
- ne pas réutiliser les contenus ;
- ne pas entraîner de modèles sur les contenus clients ;
- minimiser les métadonnées ;
- exposer uniquement les artefacts de mesure nécessaires ;
- laisser le client conserver l’autorité et le contexte.

---

## 14. Minimisation des données

L’approche de protection des données de NeoMundi repose sur la minimisation.

Seules les informations nécessaires à la production du signal de mesure runtime doivent être traitées.

Le système est conçu pour éviter la collecte inutile de :

- identifiants utilisateurs ;
- données personnelles ;
- documents métier ;
- historiques de contenus de long terme ;
- mémoire conversationnelle ;
- prompts bruts ;
- réponses brutes ;
- journaux côté client.

Lorsque le traitement sémantique est nécessaire en mode GOV, il est limité au traitement en flux transitoire.

---

## 15. Limitation de finalité

NeoMundi traite les données uniquement dans le but de produire des signaux d’observabilité et de gouvernance runtime.

La finalité n’est pas :

- l’hébergement de contenus ;
- le stockage de contenus ;
- le profilage utilisateur ;
- la publicité comportementale ;
- l’entraînement de modèles sur les contenus clients ;
- la revente de données clients ;
- la création de datasets tiers ;
- l’enrichissement de services sans rapport avec la mesure.

La finalité est la mesure.

---

## 16. Responsabilités du client

Le client reste responsable de :

- la base légale de ses propres traitements ;
- le contenu qu’il soumet à son système d’IA ;
- sa relation avec ses utilisateurs finaux ;
- sa propre notice de confidentialité ;
- sa propre configuration fournisseur LLM ;
- son propre DPA avec son fournisseur LLM lorsque applicable ;
- sa corrélation interne entre identifiants de requête et contenus ;
- ses propres politiques de conservation ;
- ses propres seuils de gouvernance ;
- ses propres décisions opérationnelles.

NeoMundi ne détermine pas la finalité métier du système d’IA du client.

NeoMundi fournit un instrument de mesure.

---

## 17. Responsabilités de NeoMundi

NeoMundi est responsable de documenter et d’opérer la couche de mesure conformément à son architecture déclarée.

Cela inclut :

- distinguer les modes OBS et GOV ;
- limiter les traitements aux finalités de mesure ;
- éviter la conservation des prompts et réponses ;
- maintenir une documentation technique claire ;
- soutenir l’auditabilité de l’instrument ;
- produire des artefacts de mesure minimaux ;
- documenter les hypothèses de privacy et de gouvernance ;
- faciliter la revue juridique et la préparation d’un DPA.

---

## 18. Positionnement juridique

Ce dépôt constitue une description technique et architecturale.

Il est conçu pour soutenir l’analyse juridique.

Il ne constitue pas :

- un DPA ;
- une politique de confidentialité ;
- un avis juridique ;
- un engagement contractuel ;
- une certification de conformité.

Un DPA formel, une notice de confidentialité ou une clause contractuelle doivent être préparés ou revus par un conseil juridique qualifié.

L’objectif de ce dépôt est de rendre l’architecture technique suffisamment claire pour permettre aux juristes, DPO, équipes sécurité et organisations clientes d’évaluer correctement le système.

---

## 19. Brief de préparation DPA

Un DPA ou addendum contractuel devrait refléter l’architecture suivante :

NeoMundi fournit deux modes de fonctionnement documentés.

### Fournisseurs tiers de mesure

Lorsque NeoMundi utilise des fournisseurs tiers pour produire des signaux de mesure spécifiques, par exemple un LLM judge, le DPA ou la documentation contractuelle devrait identifier :

- le fournisseur utilisé ;
- le type de signal produit ;
- si du contenu sémantique est transmis ;
- la finalité de la transmission ;
- les conditions applicables de conservation et d’entraînement ;
- les sous-traitants ou conditions de traitement des données applicables ;
- l’existence éventuelle d’une configuration alternative.

Au stade actuel, OpenAI peut être utilisé comme LLM judge interne pour certains signaux de mesure, selon la configuration.

### Mode OBS

En mode OBS :

- NeoMundi intervient après la génération.
- Le contenu sémantique n’est pas transmis à NeoMundi.
- Les prompts et réponses ne sont pas transmis.
- Seuls des métriques ou artefacts d’observation sont traités.

### Mode GOV

En mode GOV :

- NeoMundi intervient pendant la génération.
- Le contenu sémantique transite par NeoMundi.
- Le traitement est strictement transitoire.
- Le traitement est effectué uniquement pour produire des signaux de mesure runtime.
- Les prompts et réponses ne sont pas conservés.
- Les contenus générés ne sont pas conservés.
- Le contenu n’est pas réutilisé pour l’entraînement.

### Dans les deux modes

- Le client utilise sa propre clé fournisseur LLM.
- NeoMundi ne conserve pas les contenus.
- NeoMundi produit des artefacts minimaux.
- Le client reste responsable du contenu, de la corrélation et de la décision finale.
- NeoMundi fournit des signaux, pas des décisions juridiques ou opérationnelles finales.

---

## 20. Proposition de formulation DPA pour revue juridique

La formulation suivante peut servir de point de départ pour une revue juridique.

Elle ne doit pas être utilisée comme clause juridique finale sans validation professionnelle.

> NeoMundi fournit un instrument de mesure runtime pour les systèmes d’IA générative. Le service distingue deux modes opérationnels.
>
> En mode OBS, le service intervient après la génération et ne reçoit pas les prompts, réponses ou contenus sémantiques. Seuls des métriques, identifiants techniques ou artefacts d’observation nécessaires à la production des signaux de mesure sont traités.
>
> En mode GOV, le service intervient pendant la génération. Le contenu sémantique transite par NeoMundi uniquement pour la finalité de traitement en flux transitoire et de mesure runtime. Les prompts, réponses et contenus générés ne sont ni journalisés, ni conservés, ni indexés, ni réutilisés, ni utilisés pour l’entraînement de modèles.
>
> Dans les deux modes, le client reste responsable de sa relation avec son fournisseur LLM et utilise sa propre clé fournisseur selon une architecture BYOK. NeoMundi produit des artefacts de mesure minimaux, tels que des identifiants de requête, horodatages, mesures de stabilité, indicateurs de dérive et signaux de gouvernance.
>
> Le client reste responsable de la corrélation entre identifiants de requête, utilisateurs, contenus, contexte métier et décisions opérationnelles. NeoMundi fournit des signaux de mesure et ne remplace pas l’autorité de décision du client.

---

## 21. Considérations de sécurité et d’auditabilité

L’architecture de protection des données de NeoMundi est conçue pour soutenir l’auditabilité.

Les dimensions d’audit pertinentes incluent :

- mode utilisé : OBS ou GOV ;
- identifiant de requête ;
- horodatage ;
- signal de mesure ;
- statut de gouvernance ;
- configuration des seuils ;
- structure de l’artefact ;
- absence de conservation des prompts ;
- absence de conservation des réponses ;
- séparation entre contenu et signal.

Les futures versions de ce dépôt pourront inclure :

- schémas techniques ;
- cartographies de flux de données ;
- tableaux de conservation ;
- questionnaires sécurité ;
- sous-traitants ;
- hypothèses de déploiement ;
- exemples d’annexes DPA ;
- checklist d’audit ;
- modèles d’évaluation des risques.

---

## 22. Vue d’ensemble des flux de données

### Mode OBS

    Pile LLM du client
            ↓
    Génération terminée
            ↓
    Artefacts d’observation côté client
            ↓
    Mesure NeoMundi
            ↓
    Artefact de signal minimal
            ↓
    Couche de gouvernance client

### Mode GOV

    Application client
            ↓
    Couche de mesure runtime NeoMundi
            ↓
    Fournisseur LLM du client via BYOK
            ↓
    Génération en streaming
            ↓
    Mesure transitoire
            ↓
    Artefact de gouvernance minimal
            ↓
    Politique client / opérateur / décision système

---

## 23. Ce qu’est NeoMundi

NeoMundi est :

- un instrument de mesure runtime ;
- une couche de signal ;
- une couche d’observabilité ;
- une couche de soutien à la gouvernance ;
- un composant d’auditabilité ;
- un système de mesure à minimisation des données.

---

## 24. Ce que NeoMundi n’est pas

NeoMundi n’est pas :

- un système de stockage de contenus ;
- une base de données de prompts ;
- une base de données de réponses ;
- une plateforme d’entraînement de modèles utilisant les contenus clients ;
- un substitut au DPO du client ;
- un substitut aux obligations juridiques du client ;
- un décideur final ;
- une garantie universelle de vérité factuelle ;
- un substitut à la supervision humaine lorsqu’elle est requise.

---

## 25. Relation avec la gouvernance de l’IA

La position de NeoMundi est que la gouvernance de l’IA nécessite une mesure runtime.

Les politiques statiques sont nécessaires, mais elles ne suffisent pas.

Un système d’IA générative peut se comporter différemment selon :

- le contexte du prompt ;
- la version du modèle ;
- le comportement du fournisseur ;
- la chaîne d’orchestration ;
- les boucles agentiques ;
- le contexte de retrieval ;
- les appels d’outils ;
- l’instabilité runtime ;
- la dérive sémantique.

NeoMundi fournit des signaux qui aident les équipes à observer, documenter et gouverner ces comportements.

---

## 26. Relation avec la conformité

NeoMundi peut soutenir les workflows de conformité en produisant des artefacts de mesure runtime.

Ces artefacts peuvent aider les organisations clientes à documenter :

- l’observabilité ;
- la traçabilité ;
- la surveillance runtime ;
- les seuils de gouvernance ;
- les politiques d’escalade ;
- les pistes d’audit ;
- la supervision opérationnelle.

Cependant, NeoMundi ne certifie pas à lui seul la conformité.

La conformité dépend du système complet du client, de sa base légale, de sa politique opérationnelle, de son cadre contractuel et de ses contrôles organisationnels.

---

## 27. Questions recommandées pour la revue juridique

Lors de la préparation d’un DPA ou d’une analyse juridique, les questions suivantes devraient être examinées :

1. Dans quel mode NeoMundi est-il utilisé : OBS, GOV ou les deux ?
2. Quelles données sont transmises dans chaque mode ?
3. Le mode GOV traite-t-il des données personnelles dans le cas d’usage spécifique du client ?
4. Quelle est la base légale du cas d’usage client ?
5. Quel fournisseur LLM est utilisé par le client ?
6. Quelles sont les conditions de traitement des données propres au fournisseur LLM ?
7. Quels artefacts de mesure sont conservés par le client ?
8. Quelle corrélation le client maintient-il entre les identifiants de requête et les contenus ?
9. Quelle durée de conservation s’applique aux artefacts de mesure ?
10. Quelles décisions de gouvernance sont automatisées, assistées ou soumises à revue humaine ?
11. Quels seuils sont configurés par le client ?
12. Quelle piste d’audit est requise par le secteur du client ?
13. Quelles exigences de sécurité s’appliquent au déploiement ?
14. Quelles clauses contractuelles sont nécessaires entre les parties ?

---

## 28. Terminologie recommandée

Par cohérence, la terminologie suivante devrait être utilisée :

- instrument de mesure runtime ;
- traitement en flux transitoire ;
- artefact de mesure minimal ;
- absence de conservation des contenus ;
- architecture BYOK ;
- autorité de décision contrôlée par le client ;
- mode OBS ;
- mode GOV ;
- minimisation des données ;
- privacy by design ;
- privacy by default ;
- séparation entre contenu, signal et décision.

Éviter les formulations ambiguës telles que :

- “NeoMundi ne traite jamais de contenu” ;
- “NeoMundi est pleinement conforme par défaut” ;
- “NeoMundi garantit la conformité juridique” ;
- “NeoMundi garantit la vérité factuelle” ;
- “NeoMundi remplace la supervision humaine”.

Formulation préférée :

> En mode GOV, le contenu sémantique transite par NeoMundi uniquement pour la mesure runtime transitoire et n’est pas conservé.

---

## 29. Résumé public

NeoMundi mesure les systèmes d’IA sans conserver leurs contenus.

En mode OBS, le contenu sémantique n’est pas transmis.

En mode GOV, le contenu sémantique transite temporairement pour la mesure runtime et n’est pas stocké.

Le client conserve sa propre clé fournisseur LLM.

NeoMundi renvoie des artefacts de mesure minimaux.

Le client conserve l’autorité de décision.

---

## 30. Statut

Ce dépôt fait partie de la documentation publique NeoMundi.

Il est destiné à évoluer à mesure que l’instrument, le cadre juridique, la documentation sécurité et le modèle de gouvernance mûrissent.

Statut actuel :

Brouillon pour revue technique et juridique.  
Ne constitue pas un contrat juridique.  
Ne constitue pas un DPA final.

---

## 31. Liens

Organisation GitHub NeoMundi :  
https://github.com/neomundi-io

Cartographie LLM NeoMundi :  
https://github.com/neomundi-io/llm-cartography

Site NeoMundi :  
https://neomundi.io

Site NeoMundi Recherche :  
https://neomundi.org
