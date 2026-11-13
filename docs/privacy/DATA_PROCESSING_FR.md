<div align="center">

<sub><a href="DATA_PROCESSING.md">English</a> · <b>Français</b></sub>

# Traitement des données — catégories, finalités, flux

</div>

> **V0.1 · 13 novembre 2026 · provisoire.** Lisez d'abord
> [`README_FR.md`](README_FR.md), en particulier la distinction entre
> **fusionné** et **déployé**.

---

## 1. Catégories

| Catégorie | Ce dont il s'agit | Où elle réside |
|---|---|---|
| **Données de compte** | courriel, nom, adresse postale, numéro de TVA, téléphone, langue | base du portail |
| **Identifiants d'accès** | empreinte du mot de passe, clés API, jetons de réinitialisation | base du portail |
| **Contenu soumis** | l'invite, la réponse, et les documents joints pour l'ancrage | voir §3 — le traitement diffère selon le chemin |
| **Résultats de mesure** | scores, décisions, raisonnement, et **extraits désignés de la réponse** | base de mesure |
| **Pièces comptables** | factures, portefeuilles, transactions, virements | base du portail |
| **Journaux techniques** | identifiants de requête, durées, états d'erreur | journaux serveur |

🔴 **Les résultats de mesure ne sont pas purement numériques.** Lorsque
l'évaluateur de factualité désigne un passage comme suspect, ce passage est
conservé **verbatim**, borné à **10 extraits de 200 caractères au plus**.

⚠ Une mesure sur 927 réponses réelles a établi que **720 d'entre elles
portaient de tels extraits**, et que **69,6 %** de ces extraits provenaient
d'un repli mécanique recopiant les trois premières phrases de la réponse dès
que l'évaluateur n'avait rien désigné. **Ce repli a été retiré** — voir
[`DATA_RETENTION_FR.md`](DATA_RETENTION_FR.md) §4. **Fusionné, non
déployé.**

## 2. Finalités

| Finalité | Catégorie |
|---|---|
| Faire fonctionner le service de mesure | contenu soumis, données de compte |
| Authentification et application des quotas | identifiants, données de compte |
| Facturation | données de compte, pièces comptables |
| **Qualification scientifique des mesures** | résultats de mesure |
| Sécurité et enquête sur les abus | journaux techniques |

La finalité scientifique est la raison pour laquelle les résultats de mesure
ne sont **pas** supprimés avec un compte. Voir
[`DATA_RETENTION_FR.md`](DATA_RETENTION_FR.md) §3.

## 3. Flux — la distinction qui compte le plus

🔴 **Deux choses très différentes sont parfois appelées « envoyer des
données à un fournisseur ». Elles ne doivent pas être lues comme une seule.**

### 3.1 Les appels que **NeoMundi** initie, avec **ses** identifiants

Pour mesurer, le service peut appeler un évaluateur. Certains s'exécutent
sur une infrastructure que nous contrôlons ; d'autres non.

| Canal | Où il s'exécute | Le contenu sort-il ? |
|---|---|---|
| Stabilité | nos serveurs | non |
| Cohérence | modèle sur nos propres serveurs | non |
| Risque sémantique | service d'inférence de notre hébergeur | oui — vers notre hébergeur |
| Factualité / instabilité | service d'inférence | **canal indisponible à la date de rédaction** |
| **Ancrage documentaire** *(optionnel)* | **prestataires hors de Suisse et hors de l'Union européenne** | **oui** |

**Un garde encadre désormais ces appels.** Il combine une liste blanche
explicite et un contrôle de plausibilité de l'identifiant, et il s'applique
à **quatre** chemins de sortie. `openai` **n'est pas sur la liste blanche
par défaut**.

⚠ Le quatrième chemin — celui de la démonstration — n'a pas été trouvé en
lisant le code. Il l'a été par un **test structurel** qui analyse la source
et signale tout endroit où la simple présence d'un identifiant est traitée
comme une permission d'envoyer. *Fusionné, non déployé.*

### 3.2 Les appels que **vous** initiez, avec **vos** identifiants

Sur `POST /v1/govern/stream`, vous fournissez **votre propre clé d'API de
fournisseur**. NeoMundi appelle alors **le fournisseur que vous avez
désigné**, avec **votre** clé, et mesure la réponse au fil de l'eau.

| | |
|---|---|
| **Ce qui est transmis** | votre invite, et le contexte que vous joignez |
| **À qui** | **le fournisseur que vous désignez** — une dizaine sont reconnus, situés notamment aux 🇺🇸 États-Unis et en 🇨🇳 Chine |
| **Avec quelle clé** | **la vôtre** |
| **Qui décide** | **vous**, à chaque requête |

🔴 **La liste blanche du §3.1 ne s'applique pas ici, délibérément.** Elle
encadre les appels que *nous* initions avec *nos* identifiants. Elle n'a pas
à arbitrer un appel que vous demandez, vers un fournisseur avec lequel
**vous** êtes en relation contractuelle.

**Conséquence :** pour ce chemin, le transfert hors de Suisse et hors de
l'Union européenne **dépend de votre choix de fournisseur**, et la relation
avec lui est **la vôtre**. Nous n'avons ni contrat avec lui, ni visibilité
sur ce qu'il fait de votre contenu.

### 3.3 Une frontière qu'il vaut mieux énoncer

Le garde du §3.1 protège contre les envois vers des **fournisseurs nommés**.
Il ne contraint pas des **destinations arbitraires** : trois chemins
d'évaluateur visent un modèle interne **par défaut**, mais leur adresse est
une variable de configuration. **Une configuration peut les rendre externes
sans aucun changement de code.** Qui examine un déploiement précis devrait
confirmer ces adresses plutôt que les déduire.

## 4. Destinataires

Voir [`SUBPROCESSORS_FR.md`](SUBPROCESSORS_FR.md).

## 5. Qui répond de ces traitements

**NeoMundi Recherche**, association régie par la loi du 1er juillet 1901,
RNA `W563015824`, 29 rue Tronchet, 75008 Paris — voir
[`README_FR.md`](README_FR.md).

Cela couvre les traitements décrits sur cette page, **dans la limite de ceux
qui sont effectivement opérés**. Cela ne s'étend pas aux appels du §3.2, que
vous initiez avec vos propres identifiants vers un fournisseur avec lequel
vous êtes en relation contractuelle.

🔴 **Désigner un responsable n'est pas une appréciation de licéité.** Ces
pages décrivent une réalité technique ; elles ne sont **pas** l'information
due par un responsable au titre des articles 13 et 14 du RGPD, et elles ne
prétendent pas l'être. Cette information est un document distinct, et
l'écrire n'est pas un travail d'ingénierie. Ce qui reste ouvert figure dans
[`KNOWN_LIMITATIONS_FR.md`](KNOWN_LIMITATIONS_FR.md).
