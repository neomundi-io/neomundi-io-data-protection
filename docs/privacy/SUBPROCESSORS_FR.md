<div align="center">

<sub><a href="SUBPROCESSORS.md">English</a> · <b>Français</b></sub>

# Destinataires et sous-traitants

</div>

> **V0.1 · 13 novembre 2026 · provisoire.**
>
> 🔴 **Cette page ne liste que ce qui est établi.** Un fournisseur qui
> *pourrait* être configuré n'est pas un fournisseur qui *reçoit* des
> données, et nous ne présentons pas l'un pour l'autre.

---

## 1. Trois catégories, à ne pas confondre

| | |
|---|---|
| **A — Sous-traitants confirmés** | un contrat existe ; ils traitent sur nos instructions |
| **B — Fournisseurs que vous choisissez** | vous fournissez l'identifiant et désignez la destination ; **la relation est la vôtre** |
| **C — Configurables, non établis comme actifs** | le code peut les atteindre ; qu'un déploiement donné le fasse est une question de configuration |

## 2. Catégorie A — sous-traitant confirmé

| Fournisseur | Rôle | Localisation |
|---|---|---|
| **Infomaniak** | hébergement du service et de ses bases ; messagerie sortante ; service d'inférence utilisé par le canal de risque sémantique | 🇨🇭 Suisse |

Un accord de sous-traitance existe avec ce fournisseur.

⚠ **Une réserve, énoncée plutôt que masquée.** L'accord est signé sous une
dénomination dont le rapport aux autres entités NeoMundi n'est établi par
aucun document en notre possession. La question est ouverte, et en cours de
régularisation. Voir [`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md)
§3.

## 3. Catégorie B — les fournisseurs que **vous** choisissez

Sur `POST /v1/govern/stream`, vous fournissez votre propre clé d'API et
désignez le fournisseur. Nous appelons alors **ce** fournisseur, avec
**votre** identifiant.

| | |
|---|---|
| **Qui choisit** | vous, à chaque requête |
| **Quel identifiant** | le vôtre |
| **Qui est en relation contractuelle avec lui** | **vous** |
| **Destinations reconnues** | une dizaine, dont des fournisseurs aux 🇺🇸 États-Unis et en 🇨🇳 Chine |

🔴 **Ce ne sont pas nos sous-traitants.** Nous ne leur donnons pas
d'instruction, nous n'avons pas d'accord avec eux, et nous n'avons aucune
visibilité sur ce qu'ils font de votre contenu. **Le transfert hors de
Suisse et hors de l'Union européenne sur ce chemin dépend entièrement de
votre choix.**

Avant d'emprunter ce chemin, examinez les conditions du fournisseur que vous
comptez désigner.

## 4. Catégorie C — atteignables par configuration, non établis comme actifs

Être honnête sur cette catégorie importe plus que de la garder courte.

| Chemin | Destination par défaut | État |
|---|---|---|
| Évaluateurs d'ancrage documentaire | **Cohere** 🇨🇦 / **OpenAI** 🇺🇸 | atteignables ; `openai` **n'est pas sur la liste blanche par défaut**, `cohere` l'est |
| Évaluateurs de cohérence, risque sémantique, factualité | un modèle **interne** | **leur adresse est une variable de configuration** — un déploiement peut les pointer ailleurs |

⚠ **Le garde protège contre les envois vers des *fournisseurs nommés*, pas
contre des *destinations arbitraires*.** Trois chemins d'évaluateur visent
un modèle interne par défaut, mais rien dans le code n'empêche une
configuration de les rendre externes. **Confirmez les adresses réelles du
déploiement que vous examinez plutôt que de les déduire de cette page.**

## 5. Ce que nous ne pouvons pas vous dire

🔴 **Qu'un contenu ait été transmis à un fournisseur externe par le passé
n'est ni confirmé ni écarté.**

La liste blanche décrite ici est récente. Auparavant, le chemin d'ancrage
documentaire était structurellement armé vers un fournisseur aujourd'hui
exclu par défaut. Établir ce qui s'est réellement passé exige des
vérifications contre le service en production qui **n'ont pas** été menées.

**Un identifiant invalide ne prouve pas qu'aucun transfert n'a eu lieu.**
Nous ne ferons pas cette déduction, et rien sur cette page ne doit être lu
comme la faisant. Voir [`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md)
§2.

## 6. Paiements

Un prestataire de paiement reçoit les données de compte strictement
nécessaires au traitement d'une transaction.

⚠ L'encaissement est actuellement désactivé sur la plateforme. Nous
mentionnons ce prestataire parce que l'intégration existe, non pour
suggérer qu'elle est en usage actif.
