<div align="center">

<sub><a href="README.md">English</a> · <b>Français</b></sub>

# NeoMundi — Données & confidentialité

### Comprendre les flux de données avant d'intégrer l'IA.

**Quels contenus circulent, où les mesures sont effectuées et quelles informations restent enregistrées ou exportées : les frontières documentées de l'infrastructure NeoMundi.**

**Measurement → Station → Evidence**

[![Measurement](https://img.shields.io/badge/Measurement-Mesurer-2563eb?style=flat-square)](https://github.com/neomundi-io/neomundi-runtime-measurement/blob/main/README_FR.md)
[![Station](https://img.shields.io/badge/Station-Observer-0f766e?style=flat-square)](https://github.com/neomundi-io/neomundi-station-public/blob/main/README.fr.md)
[![Evidence](https://img.shields.io/badge/Evidence-Verifier-D4A545?style=flat-square)](https://github.com/neomundi-io/neomundi-evidence-gateway/blob/main/README.fr.md)
[![Use cases](https://img.shields.io/badge/Cas_d_usage-Explorer-64748b?style=flat-square)](https://github.com/neomundi-io/neomundi-use-cases/blob/main/README_FR.md)

**[API](https://github.com/neomundi-io/neomundi-runtime-measurement/blob/main/QUICKSTART.md)** · **[Créer un compte](https://controltower.neomundi.io/welcome)**

</div>

---

## Un principe : décrire chaque flux, sans promesse globale

NeoMundi distingue **la transmission**, **le traitement**, **la conservation** et **l'export**. Un texte absent d'un export peut avoir été transmis, traité ou conservé sur un autre parcours. Aucune garantie générale « zéro transmission », « zéro stockage » ou « zéro conservation » n'est formulée ici.

| Composant | Frontière à examiner |
|---|---|
| **Measurement / API** | Selon la requête, le prompt et la réponse peuvent être fournis ; des évaluateurs peuvent recevoir des contenus pour produire certaines mesures. |
| **Station** | Son registre interne et son interface en lecture seule n'exposent pas les mêmes données. Une interface restreinte ne démontre pas l'absence de contenu dans le registre. |
| **Evidence** | Les mécanismes de vérification d'intégrité, d'attestation et de traçabilité ne constituent pas, par eux-mêmes, une politique de confidentialité ou de conservation. Le contenu exact des artefacts exportés doit être contrôlé séparément. |

**Avant intégration :** identifier les charges utiles, destinataires, évaluateurs, journaux, sauvegardes, durées de conservation et droits d'accès du déploiement retenu. Les garanties de chaque fournisseur doivent être vérifiées séparément.

[Consulter les contributions AI Act & RGPD](https://github.com/neomundi-io/ai-act-rgpd/blob/main/README_FR.md) · [Documentation de l'API](https://github.com/neomundi-io/neomundi-runtime-measurement/blob/main/API_INTEGRATION_GUIDE.md)

**Périmètre de la revue :** constats documentaires établis le **25 octobre 2026**, non équivalents à un audit exhaustif de production. Ce dépôt n'est ni une politique de confidentialité contractuelle, ni un DPA, ni une certification.

---

**Responsable de traitement — arrêté le 13 novembre 2026.** **NeoMundi
Recherche**, association régie par la loi du 1er juillet 1901, RNA
`W563015824`, 29 rue Tronchet, 75008 Paris —
[identité publiée](https://neomundi.org/mentions-legales). Elle porte la
responsabilité des traitements de NeoMundi, **dans la limite de ceux qui
sont effectivement opérés**.

🔴 **Cela tranche qui répond, pas si les traitements sont conformes.** Cela
ne fait pas davantage de ce dépôt une politique de confidentialité — il
n'en est toujours pas une.

**→ [`docs/privacy/`](docs/privacy/README_FR.md)** — documentation technique
**V0.1**, bilingue : catégories et flux, effacement et conservation,
destinataires, et les limites qui restent ouvertes.

---

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
