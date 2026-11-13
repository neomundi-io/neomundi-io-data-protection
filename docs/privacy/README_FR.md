<div align="center">

<sub><a href="README.md">English</a> · <b>Français</b></sub>

# Protection des données — documentation technique **V0.1**

### Ce que nous avons vérifié, comment, et ce que nous n'avons pas vérifié.

</div>

---

> **Version 0.1 · 13 novembre 2026 · provisoire.**
>
> 🔴 **Ce n'est ni une politique de confidentialité, ni un DPA, ni une
> déclaration de conformité au RGPD.** C'est une description technique,
> datée et bornée, de la manière dont les données de compte sont effacées et
> de l'endroit où le contenu circule — avec ses limites nommées plutôt
> qu'omises.

---

## Pourquoi une version 0.1

Cette documentation existe parce que la page précédente de ce dépôt
([`../../README_FR.md`](../../README_FR.md), datée du 25 octobre 2026)
énonçait clairement qu'*« aucune durée de conservation universelle ni
garantie de suppression à l'échelle du déploiement n'a été établie »*.

C'est toujours vrai pour la plateforme entière. Ce qui a changé, c'est
qu'une partie — **l'effacement des comptes** — a été construite, testée et
auditée en profondeur. Ces pages décrivent ce travail, et lui seul.

## Périmètre

| Couvert | Non couvert |
|---|---|
| L'effacement d'un compte du portail, et ce qui y survit | Toute durée de conservation — aucune n'est arrêtée |
| Quel contenu sort de la machine, et vers qui | Le comportement historique du service en production |
| La différence entre appels initiés par NeoMundi et par l'utilisateur | La Station, Evidence, les sites publics |
| Ce que le code garantit, et ce qu'il ne peut pas garantir | Quelle entité juridique est responsable de traitement |

## 🔴 À lire avant tout le reste : fusionné n'est pas déployé

Le comportement d'effacement décrit dans
[`DATA_RETENTION_FR.md`](DATA_RETENTION_FR.md) a été fusionné le
**13 novembre 2026** dans la branche de la lignée de production, au commit
[`1a64aeaf`](https://github.com/neomundi-io/Backend_plateforme_v2/commit/1a64aeaff073f633f06795d61cb8e8424b90299c)
*(dépôt privé)*.

**Il n'a pas été déployé.** Aucun workflow de publication d'image n'écoute
cette branche, et aucun ne s'est déclenché. **Le service en fonctionnement
ne se comporte pas encore ainsi.**

Chaque affirmation sur l'effacement porte donc la mention **fusionné, non
déployé**. Nous préférons publier cette distinction plutôt que laisser un
lecteur supposer que le meilleur comportement est déjà en service.

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
3. **Aucune entité n'est désignée responsable de traitement ici**, parce que
   cette décision est ouverte. Sans elle, ceci ne peut pas devenir une
   politique de confidentialité.

## Les quatre pages

| | |
|---|---|
| [`DATA_PROCESSING_FR.md`](DATA_PROCESSING_FR.md) | catégories, finalités, flux, destinataires |
| [`DATA_RETENTION_FR.md`](DATA_RETENTION_FR.md) | effacement, ce qui survit, révocation des accès |
| [`SUBPROCESSORS_FR.md`](SUBPROCESSORS_FR.md) | qui reçoit quoi, et sur l'instruction de qui |
| [`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md) | ce qui est ouvert, et ce que nous n'avons pas contrôlé |

## Contact

Pour une question sur cette documentation, ouvrez une *issue* sur ce dépôt.

⚠ Nous ne publions pas ici d'adresse de contact « protection des données ».
En nommer une suppose un responsable de traitement désigné et un engagement
de réponse dans un délai réglementaire — or la question du responsable est
précisément celle qui reste ouverte *(voir
[`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md))*. Pour toute question
de compte ou de service, utilisez les canaux de votre contrat ou de
l'interface.

---

<sub>V0.1 · 13 novembre 2026 · Apache-2.0. Les descriptions antérieures
demeurent dans l'historique Git et ne constituent pas des engagements
actuels.</sub>
