<div align="center">

<sub><a href="README.md">English</a> · <b>Français</b></sub>

# Protection des données — documentation technique **V0.4**

### Ce que nous avons vérifié, comment, et ce que nous n'avons pas vérifié.

</div>

---

> **Version 0.2 · 13 novembre 2026 · provisoire.**
>
> 🔴 **Ce n'est ni une politique de confidentialité, ni un DPA, ni une
> déclaration de conformité au RGPD.** C'est une description technique,
> datée et bornée, de la manière dont les données de compte sont effacées et
> de l'endroit où le contenu circule — avec ses limites nommées plutôt
> qu'omises.

---

## Ce qui a changé en version 0.2

Trois choses, toutes des mesures plutôt que des reformulations :

1. 🟢 **Le responsable de traitement est arrêté** — NeoMundi Recherche,
   nommée ci-dessus. La version 0.1 consignait la question comme
   ouverte.
2. 🔴 **Les vérifications de transfert ont été menées.** Un transfert est
   désormais **confirmé** (Cohere, 760 mesures, 4 comptes pilotes) ; pour l'autre
   fournisseur, aucun résultat enregistré sur 340 602 mesures. Voir
   [`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md) §2.
3. 🔴 **Deux canaux d'évaluateur ne sont pas internes**, alors que la
   version 0.1 laissait entendre qu'ils l'étaient par défaut : en
   production ils visent le service d'inférence de notre hébergeur.
   Corrigé au §3.3 de
   [`DATA_PROCESSING_FR.md`](DATA_PROCESSING_FR.md).

🟢 **Et en version 0.3, la seule chose qui n'avait pas changé, a changé.**
Le comportement d'effacement est **déployé**. La production sert la révision
`fc5fadd9d212…`, vérifiée sur les conteneurs en service — voir ci-dessous.

## Pourquoi cette documentation existe

Cette documentation existe parce que la page précédente de ce dépôt
([`../../README_FR.md`](../../README_FR.md), datée du 25 octobre 2026)
énonçait clairement qu'*« aucune durée de conservation universelle ni
garantie de suppression à l'échelle du déploiement n'a été établie »*.

C'est toujours vrai pour la plateforme entière. Ce qui a changé, c'est
qu'une partie — **l'effacement des comptes** — a été construite, testée et
auditée en profondeur. Ces pages décrivent ce travail, et lui seul.

## Responsable de traitement

**NeoMundi Recherche** — association régie par la loi du 1er juillet 1901.

| | |
|---|---|
| **RNA** | `W563015824` |
| **Siège** | 29 rue Tronchet, 75008 Paris, France |
| **Identité publiée** | [neomundi.org/mentions-legales](https://neomundi.org/mentions-legales) |
| **Pour exercer vos droits** | le formulaire de contact sur [neomundi.org/contact](https://neomundi.org/contact) — aucune adresse de courriel n'est publiée |

NeoMundi Recherche porte la responsabilité des traitements de NeoMundi —
l'API de mesure, le portail, les comptes utilisateurs, les clés, les
mesures, Station, Evidence et les sites NeoMundi — **dans la limite des
traitements effectivement opérés**.

🔴 **Désigner un responsable ne rend pas les traitements conformes.** Cela
tranche qui en répond. Ce qui reste ouvert figure dans
[`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md), et c'est encore
substantiel.

## Périmètre

| Couvert | Non couvert |
|---|---|
| L'effacement d'un compte du portail, et ce qui y survit | Toute durée de conservation — aucune n'est arrêtée |
| Quel contenu sort de la machine, et vers qui | Le comportement historique du service en production |
| La différence entre appels initiés par NeoMundi et par l'utilisateur | La Station, Evidence, les sites publics |
| Ce que le code garantit, et ce qu'il ne peut pas garantir | La licéité des traitements — désigner un responsable n'est pas une appréciation |

## 🟢 Déployé le 13 novembre 2026 — et vérifié sur le service en fonctionnement

Les versions 0.1 et 0.2 de cette page disaient le comportement d'effacement
*fusionné, non déployé*, et prévenaient le lecteur de ne pas supposer que le
meilleur comportement était en service. **Il l'est désormais.**

| | |
|---|---|
| **Révision servie** | `fc5fadd9d21251712ed34a13c23803dfccc37123` |
| Services | le moteur de mesure et le portail |
| Vérifié | sur les conteneurs en service, non déduit d'une branche |
| Inchangé | le frontend web, délibérément — cette livraison n'y touche pas |

**Ce qui a été vérifié sur le service en fonctionnement**, et non sur un
candidat :

- le garde de sortie externe est actif, et refuse `openai` — `provider_not_allowed`, même présenté avec un identifiant bien formé ;
- le repli mécanique d'extraits rend une liste vide à tous les scores ;
- la seconde copie sur disque des sorties client est désactivée ;
- l'état de compte purgé existe, et l'émission de facture refuse de s'exécuter tant qu'aucun émetteur n'est arrêté ;
- `measurement_version` vaut **3.5.0**, inchangée — les résultats restent comparables à tout ce qui a été mesuré avant ;
- le corpus, les factures, les portefeuilles, les comptes et les clés d'API sont intacts, à la ligne près.

## Ce qui est réellement vérifié

| Vérifié | Comment |
|---|---|
| L'identité du compte est écrasée champ par champ | un test balaie **tous** les attributs de la ligne à la recherche de sentinelles, et non les champs qu'il attend |
| Les clés API sont supprimées — y compris révoquées | assertions SQL contre un PostgreSQL réel |
| Les jetons de réinitialisation ne survivent pas | idem |
| Les sessions sont refusées après effacement | un parcours HTTP complet : création → clé → facture → demande → purge → **tentative de réutilisation des anciens jetons** |
| L'opération est idempotente et transactionnelle | une panne est injectée en cours d'opération ; rien n'est écrit |
| Les pièces comptables survivent intactes | la facture est relue et reste lisible sans le compte |

L'ensemble est passé au vert en intégration continue : **198 tests du
portail** et **1800 tests de mesure**, zéro échec.

## Ce que nous avons manqué, et corrigé

Trois revues successives ont chacune trouvé un défaut que la précédente ne
pouvait pas voir. Nous le consignons parce que le motif est plus utile que
les correctifs.

| Périmètre de la revue | Ce qu'elle a trouvé |
|---|---|
| fonctions, en mémoire | — |
| **+ effet en base de données** | conserver la ligne de compte annulait une cascade : un compte purgé gardait une **clé API active** |
| **+ parcours HTTP réel** | un jeton de rafraîchissement émis avant la purge rendait encore un **nouveau jeton d'accès** |

🔴 **Le défaut n'était jamais dans le code regardé.** Il était dans ce que
le changement avait retiré *ailleurs*. Aucun n'a atteint la production.

## Limites

Nommées en entier dans
[`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md). Les trois principales :

1. **Aucune durée de conservation n'est arrêtée** pour les pièces
   comptables. Nous n'en inventerons pas.
2. **Qu'un contenu ait été transmis à un fournisseur externe par le passé
   n'est ni confirmé ni écarté.** Le code actuel l'interdit par défaut ;
   cela ne dit rien de l'historique.
3. **L'accord d'hébergement a été signé sous une dénomination qui n'est pas
   une entité juridique déclarée.** Le responsable est désormais arrêté ;
   l'alignement du contrat ne l'est pas, et une demande de rectification est
   préparée.

## Les quatre pages

| | |
|---|---|
| [`DATA_PROCESSING_FR.md`](DATA_PROCESSING_FR.md) | catégories, finalités, flux, destinataires |
| [`DATA_RETENTION_FR.md`](DATA_RETENTION_FR.md) | effacement, ce qui survit, révocation des accès |
| [`SUBPROCESSORS_FR.md`](SUBPROCESSORS_FR.md) | qui reçoit quoi, et sur l'instruction de qui |
| [`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md) | ce qui est ouvert, et ce que nous n'avons pas contrôlé |

## Contact

**Pour exercer un droit sur vos données personnelles**, utilisez le
formulaire de contact sur [neomundi.org/contact](https://neomundi.org/contact).
C'est le canal que NeoMundi Recherche publie ; **aucune adresse de courriel
n'est publiée**, et nous n'en inventons pas ici.

Pour une question sur **cette documentation**, ouvrez une *issue* sur ce
dépôt. Pour toute question de compte ou de service, utilisez les canaux de
votre contrat ou de l'interface.

---

<sub>V0.4 · 14 novembre 2026 · Apache-2.0. Les descriptions antérieures
demeurent dans l'historique Git et ne constituent pas des engagements
actuels.</sub>
