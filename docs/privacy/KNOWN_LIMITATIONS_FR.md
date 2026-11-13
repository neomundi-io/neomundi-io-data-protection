<div align="center">

<sub><a href="KNOWN_LIMITATIONS.md">English</a> · <b>Français</b></sub>

# Limites connues

</div>

> **V0.2 · 13 novembre 2026 · provisoire.**
>
> Cette page existe parce qu'une documentation qui n'énumère que ce qui
> fonctionne est une page commerciale. Tout ce qui suit est ouvert.

---

## 1. Fusionné n'est pas déployé — la limite qui commande toutes les autres

Le comportement d'effacement décrit dans
[`DATA_RETENTION_FR.md`](DATA_RETENTION_FR.md) a été fusionné le
13 novembre 2026 dans la branche de la lignée de production. **Il n'a pas
été déployé.**

🔴 **Le service en fonctionnement se comporte encore comme avant** : la
suppression d'un compte cascade, et cette cascade détruit les factures. Tout
ce que nous décrivons comme vérifié l'est **sur le candidat fusionné**, pas
en production.

**Ce qui lèverait cette limite :** un déploiement, qui exige une
autorisation explicite et les vérifications du §2.

## 2. Transferts externes — mesurés, et l'un est confirmé

🟢 **Les vérifications annoncées en version 0.1 ont été menées**, en lecture
seule, contre le service en production et sur **toute** l'histoire du
magasin de mesures : **340 602 mesures, du 1er mai au 13 novembre 2026**.

La version 0.1 disait « ni confirmé, ni écarté ». C'est désormais mesuré, et
les deux fournisseurs donnent des réponses **différentes**.

### Cohere — 🔴 transfert **confirmé**

| | |
|---|---|
| Mesures portant un résultat d'ancrage | **760** |
| Modèle qui les a produites | **`command-a-03-2025`** — un modèle Cohere, sur les 760 |
| Fenêtre | **10 juillet → 4 novembre 2026** |
| Clients distincts | **4** |

🔴 **Pour qu'un résultat existe, le contenu a dû être transmis.** C'est un
fait, pas une déduction. Le canal est **déclaré** au §3.1 de
[`DATA_PROCESSING_FR.md`](DATA_PROCESSING_FR.md) et il est **optionnel** :
il ne s'active que si une requête joint des documents pour l'ancrage.

⚠ **Ce qui reste ouvert, et ce n'est pas technique :** aucun accord de
sous-traitance avec ce fournisseur n'est au dossier. Le Canada bénéficie
d'une décision d'adéquation **partielle**, de portée limitée ; savoir si ce
fournisseur entre dans son champ est une appréciation **juridique** que nous
n'avons pas obtenue et que nous n'improviserons pas.

### OpenAI — 🟢 **absence établie, sur le périmètre vérifié**

| | |
|---|---|
| Mesures portant un résultat d'ancrage OpenAI | **0 sur 340 602** |
| Occurrences dans le journal du service | **0** |

🔴 **À lire précisément.** Ce qui est établi, c'est qu'**aucun résultat
d'ancrage OpenAI n'a jamais été enregistré**, sur tout le corpus. Ce qui
n'est **pas** établi, c'est qu'aucun corps de requête n'ait jamais quitté la
machine : un appel rejeté par le fournisseur ne laisse aucune trace, et le
journal du service ne couvre que depuis le **12 novembre 2026** — le
conteneur a été recréé ce jour-là.

**Un identifiant invalide n'est pas une preuve d'absence**, et nous ne le
présentons pas comme telle.

**Ce qui lèverait le reste :** que le garde de sortie atteigne la
production — voir §1.

## 3. Le responsable est arrêté ; le contrat ne l'est pas encore

✅ **Tranché le 13 novembre 2026.** Le responsable de traitement est
**NeoMundi Recherche**, association régie par la loi du 1er juillet 1901,
RNA `W563015824` — voir [`README_FR.md`](README_FR.md). Les versions
antérieures de cette page consignaient la question comme ouverte ; elle ne
l'est plus.

🔴 **Ce qui reste ouvert est plus étroit, et contractuel.** L'accord
d'hébergement conclu avec Infomaniak l'a été sous une **dénomination d'usage
qui n'est pas une entité juridique déclarée**, et ne porte **aucun
identifiant légal**. L'adresse qui y figure est celle de NeoMundi Recherche.

**Nous ne qualifions pas cet accord de nul.** Ce serait une conclusion
juridique, et nous ne l'avons pas obtenue. Ce que nous énonçons est plus
étroit : la dénomination contractuelle et le responsable désigné **ne sont
pas encore alignés**.

⚠ **Une seconde limite en découle, et c'est celle qui compte pour un
lecteur.** Désigner un responsable ne fait pas de ces pages l'information
due au titre des articles 13 et 14 du RGPD. **Une politique de
confidentialité reste à écrire** — c'est désormais un travail de rédaction
juridique, non plus une décision bloquée.

**Ce qui lèverait cette limite :** une rectification ou un avenant du
fournisseur désignant NeoMundi Recherche, et une notice d'information
rédigée avec un conseil juridique.

## 4. Aucune durée de conservation comptable

Les pièces comptables sont conservées. **Pour combien de temps n'est pas
décidé**, et cette documentation n'inventera pas de durée.

⚠ Les factures historiques ont été émises sous une dénomination qui **n'est
pas** le responsable désigné. Ces documents **ne sont pas réécrits** — une
pièce comptable émise ne se retouche pas après coup. Ils constituent une
trace historique, et ils ne confèrent aucune responsabilité actuelle à
l'entité qui y figure.

**Ce qui lèverait cette limite :** une décision documentée nommant à la fois
la durée et l'entité qui la porte.

## 5. Les surfaces que l'effacement n'atteint pas

| Surface | Nature de la limite |
|---|---|
| **Enregistrements de mesure** | conservés délibérément ; dé-liés, **pas anonymisés** — ils restent des données personnelles |
| **Empreinte du journal d'audit** | **pseudonymisation, pas anonymisation** — condensat non salé d'un identifiant peu entropique |
| **Contrats signés** | structurelle : les altérer détruit leur valeur probante |
| **Fichiers hors base de données** | atteints par une procédure d'exploitation qui **ne fait pas partie** de ce code |
| **Sauvegardes** | expirent sous 14 jours ; un effacement ne s'y propage pas avant |
| **Un fournisseur que vous avez choisi** | entièrement hors de notre portée — voir [`SUBPROCESSORS_FR.md`](SUBPROCESSORS_FR.md) §3 |

**Ce qui lèverait ces limites :** pour les deux premières, une décision sur
le maintien de la base de conservation ; pour les autres, des procédures
écrites plutôt que du code.

## 6. Limites des contrôles eux-mêmes

Être précis sur ce qu'une vérification **n'établit pas** :

- **L'intégration continue a tourné sur un candidat, pas sur la
  production.** Une chaîne verte est une preuve sur du code, pas sur un
  système en fonctionnement.
- **Les tests emploient des données synthétiques.** Aucun compte réel n'a
  été effacé, et aucune base de production n'a été touchée, à aucun moment.
- **L'environnement de test est reproductible mais non figé.** Les
  dépendances s'installent depuis des plages de versions déclarées, sans
  fichier de verrouillage.
- **Trois revues successives ont chacune trouvé un défaut que la précédente
  ne pouvait pas voir** — en élargissant le périmètre des fonctions, à leur
  effet en base, puis à un parcours HTTP réel. Ce périmètre est désormais
  couvert aux trois niveaux. **Ce n'est pas une affirmation qu'il ne reste
  aucun défaut.**

## 7. Aucune déclaration globale de conformité au RGPD

🔴 **NeoMundi n'est pas présenté ici comme conforme au RGPD**, et rien dans
ces pages ne doit être lu comme une telle déclaration.

Ce que cette documentation établit est plus étroit et, croyons-nous, plus
utile : une description datée et vérifiable de comportements techniques
précis, dont chaque frontière est nommée.

La conformité est une appréciation juridique qui dépend des §3, §4 et §5 —
dont aucun ne se referme par de l'ingénierie seule.

---

## Signaler un problème

Si vous estimez que cette documentation est inexacte, ouvrez une *issue* sur
ce dépôt.

Pour un sujet de **sécurité**, n'ouvrez pas d'*issue* publique. Passez par
votre contact contractuel, et décrivez le problème sans publier les étapes
qui permettraient à quelqu'un de le reproduire.

⚠ Cette organisation ne publie pas encore de politique de divulgation
coordonnée. C'est en soi une lacune, et elle figure ici plutôt que d'être
passée sous silence.
