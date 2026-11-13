<div align="center">

<sub><a href="SUBPROCESSORS.md">English</a> · <b>Français</b></sub>

# Destinataires et sous-traitants

</div>

> **V0.4 · 14 novembre 2026 · provisoire.**
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

Un accord de sous-traitance existe avec ce fournisseur, conclu par voie
électronique le **13 novembre 2026**, dans lequel le client est désigné
responsable de traitement et Infomaniak Network SA sous-traitant.

⚠ **Une réserve, énoncée plutôt que masquée.** Cet accord nomme le client
par une **dénomination d'usage qui n'est pas une entité juridique
déclarée**, et ne porte **aucun identifiant légal** — ni numéro
d'enregistrement, ni forme juridique. L'adresse qui y figure est celle de
NeoMundi Recherche.

🔴 **Nous n'en concluons pas que l'accord serait nul** — c'est une
appréciation juridique, et nous ne l'avons pas obtenue. Nous en concluons
que la dénomination contractuelle et le responsable désigné **ne sont pas
encore alignés**, et une demande de rectification a été préparée auprès du
fournisseur. Voir [`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md) §3.

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

| Chemin | État, **mesuré le 13 novembre 2026** |
|---|---|
| **Cohere** 🇨🇦 — ancrage documentaire | 🔴 **passe en catégorie B bis : le transfert est confirmé.** 760 mesures, 4 comptes pilotes, juillet→novembre 2026. Voir [`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md) §2 |
| **OpenAI** 🇺🇸 — ancrage documentaire | 🟢 **aucun résultat jamais enregistré**, sur 340 602 mesures. Hors liste blanche par défaut, et le garde déployé le refuse |
| Cohérence, risque sémantique | 🔴 **visent en réalité le service d'inférence de notre hébergeur** en production, pas un modèle interne |
| Factualité | 🔴 **vise également le service d'inférence de notre hébergeur** en production. La version 0.2 de cette page décrivait ce canal comme indisponible, sur la foi d'une sonde de supervision ; la sonde était fausse. Remesuré le 13 novembre 2026 : **1831 mesures sur 1841** en sept jours portent un signal de factualité mesuré, `evaluator_runtime=api.infomaniak.com`, `served_by=primary` |

⚠ **Le garde protège contre les envois vers des *fournisseurs nommés*, pas
contre des *destinations arbitraires*.** L'adresse d'un évaluateur est une
variable de configuration, et la configuration déployée **n'est pas** le
défaut du code — la troisième ligne le montre. **Lisez les adresses réelles
du déploiement que vous examinez plutôt que de les déduire de cette
page.**

## 5. Ce que nous savons désormais, et ce que nous ne pouvons toujours pas dire

La version 0.1 de cette page disait que les transferts étaient « ni
confirmés ni écartés ». **Les vérifications ont depuis été menées**, en
lecture seule, sur toute l'histoire du magasin de mesures.

🔴 **Un transfert est confirmé :** 760 mesures ont été ancrées par Cohere
entre juillet et novembre 2026, pour 4 comptes pilotes. Le §3 ci-dessus et
[`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md) §2 portent les
chiffres.

🟢 **Pour OpenAI, aucun résultat d'ancrage n'a jamais été enregistré** —
zéro sur 340 602 mesures.

**Ce que nous ne pouvons toujours pas vous dire**, et nous ne prétendrons
pas le contraire : si un corps de requête a jamais quitté la machine vers un
fournisseur qui l'a ensuite rejeté. Un appel rejeté ne laisse aucune trace,
et le journal du service ne couvre que depuis le 12 novembre 2026. **Un
identifiant invalide ne prouve pas qu'aucun transfert n'a eu lieu**, et rien
sur cette page ne doit être lu comme faisant cette déduction.

## 6. Paiements

Un prestataire de paiement reçoit les données de compte strictement
nécessaires au traitement d'une transaction.

⚠ L'encaissement est actuellement désactivé sur la plateforme. Nous
mentionnons ce prestataire parce que l'intégration existe, non pour
suggérer qu'elle est en usage actif.
