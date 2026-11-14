<div align="center">

<sub><a href="DATA_RETENTION.md">English</a> · <b>Français</b></sub>

# Effacement et conservation

</div>

> **V0.5 · 14 novembre 2026 · provisoire.**
>
> 🟢 **Tout ce qui figure aux sections 2 à 5 est DÉPLOYÉ.** La production
> sert la révision `fc5fadd9d212…` depuis le 13 novembre 2026, vérifiée sur
> les conteneurs en service. Les versions 0.1 et 0.2 de cette page disaient
> le contraire, et le disaient à dessein : le comportement était alors
> fusionné mais pas en service.

---

## 1. Le principe : trois strates, pas une opération

Supprimer un compte était indivisible : un `DELETE`, et une cascade
emportait tout — **y compris les factures**.

C'est faux dans les deux sens. Cela détruisait des pièces que la loi impose
de conserver, et la première tentative de correction est allée trop loin en
sens inverse : elle *refusait l'effacement entier* dès qu'une facture
existait, rendant **ineffaçable tout compte ayant été facturé une fois**. Un
droit qui ne s'exerce jamais n'existe pas.

🔴 **Les deux objectifs cessent d'être en concurrence dès qu'on remarque un
fait : une facture porte sa propre copie du nom, de l'adresse, du numéro de
TVA et du courriel, figée à l'émission. La pièce comptable, c'est la
facture, pas la ligne de compte.**

Vider la ligne de compte ne retire donc rien à la comptabilité — et ne pas
la supprimer arrête la cascade.

| Strate | Traitement | Fondement |
|---|---|---|
| **Données personnelles** | **effacées, toujours** | rien ne les retient |
| **Pièces comptables** | **conservées, intactes** | obligation légale de conservation |
| **Résultats de mesure** | **conservés, dé-liés** | finalité scientifique documentée |

## 2. Ce qui est effacé

| Donnée | Traitement |
|---|---|
| Nom, prénom, adresse, code postal, ville | écrasés par un marqueur constant |
| Société, numéro de TVA, téléphone, fonction, remarque libre | mis à nul |
| **Empreinte du mot de passe** | écrasée — **aucun mot de passe n'authentifie ensuite**, mesuré |
| Courriel | remplacé par une adresse inutilisable, propre à la ligne |
| **Code de parrainage** | libéré — il est unique, et contient souvent un prénom |
| **Clés API** | **supprimées — y compris révoquées** |
| **Événements d'usage** | supprimés |
| **Jetons de réinitialisation de mot de passe** | supprimés |
| Usages de codes promotionnels | supprimés |
| Dossier d'inscription en attente | anonymisé, et son lien au compte coupé |

🔴 **Les sessions sont refusées.** Deux verrous indépendants : un contrôle
de statut sur toutes les routes porteuses *et* sur le rafraîchissement de
jeton, adossé à la base ; et l'invalidation des jetons déjà émis, appliquée
au mieux, pour qu'une panne d'infrastructure ne puisse jamais empêcher un
effacement.

**Pourquoi deux verrous.** Tant que l'effacement supprimait la ligne de
compte, toutes les frontières d'authentification tenaient pour une raison
implicite : le compte n'était simplement plus là. Conserver la ligne pour
protéger la comptabilité retire ce filet, partout à la fois. Nous l'avons
découvert par un test de bout en bout : un jeton de rafraîchissement émis
avant la purge rendait encore un **nouveau jeton d'accès**. Cela n'a jamais
atteint la production.

**Garanties vérifiées par test :** idempotence · transactionnalité *(une
panne injectée en cours d'opération n'écrit rien)* · une purge rejouée est
refusée · l'adresse d'origine redevient disponible.

## 3. Ce qui est conservé, et pourquoi

### 3.1 Les pièces comptables

Factures, portefeuilles et leurs transactions, demandes de virement,
commissions et retraits d'affiliation sont **conservés intacts**.

🔴 **Aucune durée de conservation n'est indiquée ici, parce qu'aucune n'a
été arrêtée.** Nous n'en inventerons pas. Tant que cette décision n'est pas
consignée, les pièces sont conservées — conserver est réversible, détruire
ne l'est pas.

### 3.2 Les résultats de mesure

Les résultats de mesure ne sont **pas** supprimés avec un compte. Les
détruire détruirait le corpus que les mesures existent pour constituer.

Ce qui change, c'est le lien : le dossier d'inscription est anonymisé et sa
référence au compte effacée, de sorte que la jointure qui menait d'un
extrait conservé à une personne nommée est rompue.

⚠ **C'est une dé-liaison, pas une anonymisation.** Ces enregistrements
restent des données personnelles au sens du RGPD, conservées sur une base
documentée.

## 4. Les extraits de votre réponse

Une mesure peut conserver des extraits **désignés** de la réponse — **10**
au plus, de **200 caractères** au plus chacun. Jamais l'invite. Jamais la
réponse entière.

Un repli mécanique recopiait les **trois premières phrases de la réponse**
dès que l'évaluateur n'avait rien désigné. Sur 927 réponses réelles, ce
repli produisait **69,6 %** de tous les extraits conservés. **Il a été
retiré.** L'ouverture d'une réponse n'est pas un passage suspect : c'est
juste son ouverture. *Déployé et vérifié le 13 novembre 2026.*

🔴 **Les extraits déjà conservés n'ont pas été modifiés.** Les traiter est
une décision distincte, non prise ici, et un effacement naïf détruirait la
provenance qui rend une mesure re-vérifiable.

## 5. L'empreinte du journal d'audit — pseudonymisation, pas anonymisation

L'effacement inscrit dans le journal d'audit une empreinte à sens unique de
l'adresse, afin de pouvoir répondre plus tard à « avez-vous effacé
alice@exemple.fr ? » sans conserver l'adresse.

🔴 **C'est une pseudonymisation. Ce n'est pas une anonymisation, et nous
préférons le dire.** Il s'agit d'un SHA-256 **non salé** d'une adresse de
courriel. Les adresses sont peu entropiques : qui dispose d'une liste
d'adresses retrouve celle-ci par comparaison, et le domaine est conservé à
côté, ce qui réduit encore la recherche. **Au sens du RGPD, cette empreinte
reste une donnée personnelle.**

Le choix est assumé, non subi : la saler ou la chiffrer la rendrait
invérifiable par la seule personne fondée à poser la question — ce à quoi
elle sert précisément.

## 6. Ce que l'effacement n'atteint **pas**

| Surface | Pourquoi |
|---|---|
| **Les enregistrements de mesure** | conservés délibérément, dé-liés — §3.2 |
| **L'empreinte du journal d'audit** | pseudonymisée, conservée — §5 |
| **Les contrats signés** | les altérer invaliderait la signature |
| **Les fichiers hors base de données** | atteints par une procédure d'exploitation, pas par ce code |
| **Les sauvegardes de base** | elles expirent d'elles-mêmes, **sous 14 jours** |
| **Un fournisseur que vous avez choisi** | la relation est la vôtre — voir [`SUBPROCESSORS_FR.md`](SUBPROCESSORS_FR.md) |

🔴 **Nous n'affirmons pas que l'effacement est complet.** Six surfaces
échappent à ce que le code peut garantir, et les énumérer est plus utile
qu'une phrase laissant entendre le contraire.
