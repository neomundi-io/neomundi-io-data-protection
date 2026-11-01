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

## Constats complémentaires — 1er novembre 2026

Ces précisions proviennent des audits de déploiement MCP et de l'API communiqués lors de la qualification du plugin. Elles décrivent **le périmètre examiné**, pas une garantie universelle pour toute installation NeoMundi.

### Connecteur MCP hébergé

- Le service `mcp.neomundi.org` est annoncé déployé en version connecteur `0.3.0-rc.1` sur un VPS Infomaniak, avec point d'entrée HTTPS et transport MCP Streamable HTTP. Cette version **ne doit pas être confondue** avec la version de mesure renvoyée par l'API (notamment `3.3.1` dans l'observation examinée).
- En mode mesure, le connecteur transmet les données nécessaires à l'API de mesure : le texte soumis peut inclure prompt, réponse et autres éléments d'exécution. Le connecteur restitue l'enveloppe de résultat ; il ne calcule pas lui-même les scores.
- Une réponse peut comporter des signaux mesurés, des champs non publiés, des éléments textuels comme `suspect_phrases`, un état de mesure, un verdict technique et des informations de provenance. **Un champ non publié n'est pas nécessairement non mesuré** ; inversement, l'absence de mesure ne doit jamais devenir une valeur favorable par défaut.
- Les verdicts `ALLOW`, `FLAG` et `BLOCK`, lorsqu'ils sont présents, ne constituent ni certification, ni preuve de vérité, ni autorisation générale d'exécuter une action. La comparabilité des résultats dépend des versions et des changements précisément documentés.

### Journalisation et conservation : périmètres distincts

Les audits transmis font état des constats suivants, qui ne sont **pas** des engagements de conservation généralisables :

| Périmètre examiné | Observation rapportée | Limite |
|---|---|---|
| Connecteur MCP | Aucune conservation applicative de prompt ou réponse identifiée dans le connecteur audité | Ne préjuge ni de l'API, ni du client, ni du fournisseur d'IA |
| Journaux d'accès nginx de l'API | IP et User-Agent présents ; durée rapportée de **14 jours** | Dépend de la configuration effective du service |
| Journaux de conteneurs / journald de l'API | Aucune rotation configurée dans le périmètre examiné | Aucune durée maximale démontrée |
| Journaux de l'API examinés | Aucun prompt ni réponse retrouvé dans l'échantillon de **5 000 lignes** | Un échantillon négatif ne prouve pas l'absence de tout stockage |
| Base de mesures et sauvegardes | Règles de conservation non complètement établies | Aucune durée ni suppression automatique ne doit être présumée |

Les journaux techniques, les métriques, le texte d'analyse et les preuves de provenance sont **des catégories différentes**. La durée de 14 jours ci-dessus ne s'applique **pas** à toutes les observations scientifiques ou aux comptes pilotes.

### Évaluateurs et prestataires

Sur le déploiement de mesure audité, trois des cinq signaux examinés utilisent des évaluateurs locaux ; un parcours de factualité utilise un service d'inférence Infomaniak. Il ne faut pas déduire la destination effective des données du seul nom d'un champ, de la présence d'une clé ou d'un format d'API compatible OpenAI. La présence du nom `cohere` dans un champ n'établit pas à elle seule un appel à Cohere.

L'hébergement examiné est situé en Suisse chez Infomaniak. Les conditions contractuelles, les éventuels sous-traitants et la conservation par les services d'inférence doivent être vérifiés pour le **service et le compte concernés**. Une demande de rectification des coordonnées de l'entité contractante Infomaniak est en cours au 1er novembre 2026.

### Responsabilités et documents

L'association NeoMundi Recherche porte la phase pilote du connecteur. Les responsabilités de traitement des données personnelles doivent être qualifiées **par opération et par finalité** ; un pilote peut être responsable de certains traitements, et NeoMundi de ses traitements propres, selon les faits. Un NDA ou des CGU n'écartent pas les obligations d'information RGPD.

- [Mentions légales](https://neomundi.org/mentions-legales)
- [Politique de confidentialité du site](https://neomundi.org/politique-de-confidentialite) — son extension explicite au MCP est en cours
- [CGU FR](https://neomundi.org/cgu) et [EN](https://neomundi.org/en/terms-of-use) — vérifier leur statut de publication et la version applicable avant de les invoquer

Les conventions pilotes, DPA, licences d'exploitation et pièces associatives ne sont **pas** publiés dans ce dépôt. Les informations ci-dessus ne constituent ni un avis juridique, ni une certification de conformité, ni une promesse d'absence de données personnelles.

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
