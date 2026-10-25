# Données et confidentialité
[English](README.md) · [Français](README_FR.md)

NeoMundi comporte plusieurs flux de données. Une affirmation sur un export ne
décrit pas tous les traitements de l'API de mesure ni les enregistrements de Station.

## Ce qui circule
| Parcours | Ce que montre l'implémentation examinée | Frontière |
|---|---|---|
| SDK Python → API de mesure | Le client inclut `llm_prompt` et `llm_response` lorsque l'appelant les fournit. Ces champs sont optionnels dans le client examiné. | Leur omission ne prouve ni calcul local de toutes les mesures sémantiques ni disponibilité de tous les contrôles. |
| MCP → API de mesure | En observation, l'adaptateur transmet l'objet d'exécution fourni. En génération, il envoie les paramètres et l'identifiant fournisseur configuré au service amont. | Transport MCP et transport vers la mesure sont distincts. Le connecteur ne calcule pas lui-même les mesures. |
| Service de mesure → évaluateurs | Le texte soumis peut être utilisé pour les analyses sémantiques selon la route, la configuration et les contrôles activés. | Identifier les destinations effectives et conditions fournisseurs du déploiement ; ne pas les déduire des seuls noms OBS ou GOV. |
| Enregistrements Station | La documentation publique décrit la conservation des réponses et du contexte de mesure dans le registre interne. | C'est distinct de ce que restitue l'interface en lecture seule. |
| Lecture seule Station | Le contrat public décrit une restitution d'empreintes et de longueurs, plutôt que des invites et réponses brutes. | Une interface de lecture restreinte ne prouve pas l'absence de traitement ou de stockage du texte en amont. |
| Export d'interopérabilité | Le contrat d'échange définit ses propres champs et frontières de divulgation. | Un export sans texte brut ne signifie pas que tous les flux internes en sont dépourvus. |

## Transmettre et conserver sont deux choses
Cette page n'affirme pas une absence générale de transmission ou de conservation.
La conservation doit être spécifiée pour chaque stockage, service, journal,
sauvegarde et évaluateur selon la configuration effectivement déployée.

Le code API local examiné contient des contrôles destinés à exclure le texte brut
d'un chemin de journalisation des mesures. Ce constat limité sur l'implémentation
n'est ni un audit complet des stockages de production ni une garantie portant
sur Station, les fournisseurs, toutes les routes, les sauvegardes ou tous les journaux.

Aucune durée universelle de conservation ni garantie globale d'effacement
n'a été établie par cette revue documentaire.

## Limiter les données envoyées
Transmettre uniquement ce que demande la mesure choisie. Ne pas placer de clés
dans les invites ou le contenu d'exécution ; utiliser les canaux d'authentification prévus.
Un futur mode limité aux métriques devra préciser où chaque métrique est calculée,
les contrôles encore disponibles et la provenance conservée.

Générer chez le fournisseur depuis le client ne signifie pas, à lui seul, que
la réponse reste ensuite sur le client : une soumission pour mesure peut inclure ce texte.

## Préparer une intégration
Pour le déploiement choisi, établir les charges utiles, destinations, évaluateurs
activés, stockages, règles de conservation et d'effacement, contrôles d'accès et
responsabilités contractuelles. Vérifier la séparation des données et les accès
autorisés avant tout partage de dossiers pilotes.

Cette page décrit des frontières techniques ; ce n'est ni un DPA, ni un contrat
juridique, ni une certification.

## Sources et état de la revue
Revue documentaire du 25 octobre 2026. Aucun nouveau trafic de production capturé
pour cette page ; politiques de conservation des évaluateurs non auditées indépendamment.

- [Construction des requêtes SDK](https://github.com/neomundi-io/neomundi-python-sdk/blob/main/src/neomundi/client.py)
- [Adaptateur MCP](https://github.com/neomundi-io/neomundi-measurement-mcp/blob/main/runtime/adapter.mjs)
- [Documentation API](https://github.com/neomundi-io/neomundi-runtime-measurement)
- [Frontières publiées de Station](https://github.com/neomundi-io/neomundi-station-public)
- [Contrat d'échange](https://github.com/neomundi-io/neomundi-measurement-interoperability)

Les anciennes descriptions restent dans l'historique Git et ne constituent pas
un engagement général actuel de confidentialité.
