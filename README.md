# A9 Tableau de bord

Application web autonome de suivi des modifications de paramètres du process d'injection réalisées pendant une production.

Le projet **A9 Tableau de bord** permet au régleur de déclarer une intervention, de saisir les valeurs avant et après modification, de contrôler le respect du remplissage dynamique et de générer deux fichiers JSON : un export A9 détaillé et un export simplifié destiné à l'intégration dans une base SQL par le fournisseur.

## Statut du projet

- **Version fonctionnelle :** 1.1
- **Application principale :** `index.html`
- **Technologies :** HTML5, CSS3 et JavaScript natif
- **Dépendances :** aucune
- **Hébergement possible :** GitHub Pages ou serveur web statique
- **Navigateurs recommandés :** Google Chrome ou Microsoft Edge

## Objectifs

- Identifier l'ordre de fabrication concerné.
- Identifier le régleur par son matricule.
- Déclarer le type de rebut et un commentaire éventuel.
- Enregistrer les paramètres réellement modifiés.
- Conserver les valeurs avant et après modification.
- Gérer les paliers, zones de température et thermos ajoutés dynamiquement.
- Vérifier le respect du remplissage dynamique avant l'enregistrement.
- Afficher un historique détaillé pendant la session.
- Générer deux fichiers JSON destinés à des usages différents.
- Faciliter un déploiement identique sur plusieurs sites CLAYENS.

## Utilisation

### 1. Identification

Au démarrage, l'utilisateur renseigne :

- le numéro d'OF ;
- le matricule du régleur.

#### Format du numéro d'OF

Le format est contrôlé selon la règle suivante :

```text
2 lettres + 8 chiffres + / + 2 chiffres
```

Exemple :

```text
FF26201122/20
```

Le séparateur `/` est ajouté automatiquement pendant la saisie.

#### Matricule du régleur

Le champ **Matricule régleur** :

- est obligatoire ;
- accepte uniquement des chiffres ;
- supprime automatiquement les lettres, espaces et caractères spéciaux ;
- utilise un clavier numérique sur les terminaux compatibles.

Exemple :

```text
12345
```

Le matricule est ensuite affiché dans l'intervention, dans l'historique et dans les deux exports JSON.

### 2. Déclaration du rebut

Le régleur sélectionne un type de rebut parmi la liste :

- Bavure
- Retassure
- Incomplète
- Aspect
- Pollution
- Ligne de soudure
- Déformation
- Insert NC
- Coloration
- Autre

Un commentaire général facultatif peut être ajouté.

Lorsque **Autre** est sélectionné, une fenêtre demande obligatoirement une précision complémentaire.

## Organisation des paramètres

Les paramètres sont répartis dans trois onglets principaux :

- ⚙️ Process injection
- 🌡️ Températures
- 🏭 Moule

### Process injection

Sous-onglets :

- 🧪 Dosage
- ⚡ Dynamique
- ⏱️ Maintien

#### Dosage

| Paramètre | Saisie | Unité(s) |
|---|---|---|
| Course de dosage | Avant / Après | mm ou cm³ |
| Vitesse de rotation | Paliers ajoutables | Tr/min ou m/s |
| Contre-pression | Avant / Après | bar ou bar spécifique |
| Décompression | Avant / Après | mm ou cm³/s |
| Temps de refroidissement | Avant / Après | secondes |

La **Course de dosage** est affichée en première position.

#### Dynamique

| Paramètre | Saisie | Unité(s) |
|---|---|---|
| Vitesse | Paliers ajoutables | mm/s ou cm³/s |
| Course | Paliers ajoutables | mm ou cm³ |
| Temps d'injection | Avant / Après | secondes |
| Point de commutation en course | Avant / Après | mm ou cm³ |
| Point de commutation en pression | Avant / Après | bar ou bar spécifique |

#### Maintien

| Paramètre | Saisie | Unité(s) |
|---|---|---|
| Pression (Maintien) | Paliers ajoutables | bar ou bar spécifique |
| Temps (Maintien) | Paliers ajoutables | secondes |
| Matelas | Avant / Après | mm ou cm³ |

### Températures

Sous-onglets :

- 🔥 Fourreau
- ♨️ Bloc chaud

#### Température fourreau

- Buse
- Zone 1 à Zone 6 par défaut
- Zones supplémentaires ajoutables
- Valeurs Avant / Après
- Unité : °C

#### Température bloc chaud

- Zone 1 à Zone 16 par défaut
- Zones supplémentaires ajoutables
- Valeurs Avant / Après
- Unité : °C

### Moule

| Paramètre | Saisie | Unité(s) |
|---|---|---|
| Force de verrouillage | Avant / Après | Tonne ou KN |
| Température partie fixe | Thermos ajoutables | °C |
| Température partie mobile | Thermos ajoutables | °C |

Un thermo est affiché par défaut pour chaque partie du moule. Des thermos supplémentaires peuvent être ajoutés.

## Règle de saisie Avant / Après

Une valeur **Après** ne peut pas être saisie tant que la valeur **Avant** correspondante n'a pas été renseignée.

Comportement :

1. Le champ Après est désactivé par défaut.
2. La saisie de la valeur Avant déverrouille le champ Après.
3. Si la valeur Avant est effacée, le champ Après est vidé et reverrouillé.
4. La règle s'applique aux paramètres simples, paliers, zones et thermos.

## Avertissement sur les paramètres majeurs

Le message suivant est affiché au-dessus des onglets :

> ⚠️ **Attention :** les paramètres majeurs suivants : **temps d'injection, température matière, outillages, pression de maintien et tolérances matelas** ne peuvent faire l'objet d'aucune modification autre que dans les tolérances définies sans validation interne, qualité, fabrication et externe client. **(Si exigence contractuelle)**

Cet avertissement est informatif et ne bloque pas la saisie.

## Contrôle du remplissage dynamique

Lors du clic sur **Enregistrer**, une fenêtre obligatoire demande :

```text
Remplissage dynamique respecté ?
```

Réponses possibles :

| Réponse | Signification | Code export SQL |
|---|---|---:|
| C | Conforme | 1 |
| NC | Non conforme | 0 |
| NA | Non applicable | 2 |

La fenêtre contient également le message :

> Si le remplissage dynamique n'est pas respecté, alerter le service qualité et le chef d'équipe pour décision.

L'intervention n'est pas enregistrée tant qu'une réponse n'a pas été sélectionnée.

## Historique de session

Après l'enregistrement, l'historique affiche :

- le numéro d'OF ;
- le type de rebut ;
- le matricule du régleur ;
- la réponse au remplissage dynamique ;
- chaque paramètre modifié ;
- le palier, la zone ou le thermo concerné ;
- la valeur Avant avec son unité ;
- la valeur Après avec son unité ;
- les noms des deux fichiers JSON générés ;
- la date et l'heure de l'intervention.

Exemple :

```text
OF FF26100011/20 - Bavure
Matricule régleur : 12345
Remplissage dynamique : C

Course de dosage
Avant : 100 mm
Après : 150 mm
```

L'historique est stocké uniquement dans la mémoire de la page. Il est perdu lors d'une actualisation ou de la fermeture du navigateur.

## Exports JSON

Chaque enregistrement produit deux fichiers JSON dans le même dossier.

### Export JSON A9 détaillé

Nom du fichier :

```text
export_A9_NUMERO_OF_HORODATAGE.json
```

Exemple :

```json
{
  "NumeroOF": "FF26100011/20",
  "MATRICULE_REGLEUR": "12345",
  "RemplissageDynamique": "C",
  "ParametresModifies": {
    "Course de dosage": {
      "Avant": "100",
      "Après": "150",
      "unite": "mm"
    }
  }
}
```

Cet export conserve la structure complète des modifications A9.

### Export JSON destiné à l'intégration SQL

Nom du fichier :

```text
export_SQL_NUMERO_OF_HORODATAGE.json
```

Exemple :

```json
{
  "OF_REFOF": "FF26100011/20",
  "MATRICULE_REGLEUR": "12345",
  "DATE_SAISIE": "2026-09-04T14:11:52.736Z",
  "VERSION_A9": "1.1",
  "INTER_Anomalie": "Bavure - Ligne de joint visible",
  "INTER_Action": "Course de dosage valeur avant 100 mm valeur après 150 mm",
  "INTER_RemplissageDyn": 1
}
```

#### Construction de `INTER_Anomalie`

Le champ est constitué de :

```text
Type de rebut - précision Autre éventuelle - commentaire général éventuel
```

Exemples :

```text
Bavure - Ligne de joint visible
Autre - Marbrure sur face visible - Apparition après redémarrage
```

#### Construction de `INTER_Action`

Toutes les modifications sont converties en texte lisible et séparées par ` ; `.

Exemple :

```text
Course de dosage valeur avant 100 mm valeur après 150 mm ; Pression (Maintien) Palier 1 valeur avant 450 bar spécifique valeur après 470 bar spécifique
```

Les paramètres simples, paliers, zones et thermos sont pris en charge.

## Choix du dossier d'enregistrement

Lorsque le navigateur prend en charge la **File System Access API**, l'application demande de sélectionner un dossier lors du premier enregistrement.

- Les deux fichiers JSON sont enregistrés dans ce dossier.
- Le même dossier est réutilisé pendant la session.
- Le dossier actif est affiché dans l'application.

Si la sélection directe d'un dossier n'est pas disponible ou si elle est annulée, le navigateur télécharge les deux fichiers séparément.

### Recommandation Chrome

Google Chrome a été validé avec la génération des deux fichiers JSON.

Selon la configuration du navigateur, il peut être nécessaire d'autoriser les téléchargements multiples pour le site A9.

## Nommage des fichiers

Le numéro d'OF et l'horodatage sont intégrés dans les noms de fichiers.

Exemples :

```text
export_A9_FF26100011_20_2026-09-04T14-11-52-736Z.json
export_SQL_FF26100011_20_2026-09-04T14-11-52-736Z.json
```

Les caractères non compatibles avec les noms de fichiers sont remplacés par `_`.

## Charte graphique

Palette actuelle :

| Usage | Couleur |
|---|---|
| Vert pétrole principal | `#24575D` |
| Orange d'action | `#FF8500` |
| Orange au survol | `#E87600` |
| Fond général | `#F4F6F8` |
| Bordures | `#D7DDE3` |
| Champs désactivés | `#EEF1F3` |
| Texte désactivé | `#98A2B3` |
| Texte secondaire | `#667085` |

Les pictogrammes sont utilisés uniquement dans les onglets principaux et les sous-onglets.

## Installation et hébergement

L'application est autonome. Il suffit de mettre à disposition :

```text
index.html
```

L'application peut être :

- ouverte localement dans un navigateur ;
- publiée sur un serveur web statique ;
- hébergée sur GitHub Pages.

### Dépôt GitHub

```text
https://github.com/VGRIGI/A9-Tableau-de-bord
```

### Adresse GitHub Pages prévue

```text
https://vgrigi.github.io/A9-Tableau-de-bord/
```

## Mise à jour sur GitHub

1. Télécharger la nouvelle version du fichier HTML.
2. Renommer le fichier en `index.html` si nécessaire.
3. Ouvrir le dépôt GitHub.
4. Remplacer le fichier `index.html` existant.
5. Créer un commit sur la branche `main`.
6. Attendre la republication de GitHub Pages.
7. Utiliser `Ctrl + F5` pour actualiser sans cache.

## Structure du dépôt

```text
A9-Tableau-de-bord/
├── index.html
└── README.md
```

## Limites actuelles

- Pas de base de données intégrée dans A9.
- Pas d'appel direct à une API SQL.
- Historique non persistant après fermeture de la page.
- Matricule saisi manuellement et non contrôlé dans une base RH.
- Pas de connexion directe à SIDAQ ou Cyclades.
- Le fournisseur doit prendre en charge la lecture du second JSON et l'intégration SQL.
- Le dossier sélectionné est conservé uniquement pendant la session du navigateur.
- Un hébergement GitHub Pages public expose le code source de l'application.

## Stratégie d'intégration multi-sites

Le déploiement multi-sites repose sur une approche par fichiers :

```text
A9 Tableau de bord
├── JSON A9 détaillé
└── JSON SQL simplifié
    └── traitement fournisseur
        └── insertion dans la base locale du site
```

Cette stratégie évite de déployer sur chaque site :

- une API spécifique ;
- un site IIS applicatif ;
- des comptes de service SQL ;
- des certificats et règles réseau supplémentaires.

Le fournisseur est responsable de l'intégration du second JSON et de la duplication de la solution sur les différents sites.

## Évolutions possibles

- Contrôle du matricule dans une base RH.
- Lecture du matricule par badge.
- Lecture de l'OF par code-barres ou QR code.
- Stockage permanent de l'historique.
- Envoi automatique des fichiers vers un dossier réseau.
- Ajout d'un accusé de traitement du fournisseur.
- Ajout d'un identifiant unique d'intervention.
- Gestion de version du schéma JSON.
- Exploitation des données dans Power BI.

## Fichiers du projet

| Fichier | Rôle |
|---|---|
| `index.html` | Application A9 complète |
| `README.md` | Documentation fonctionnelle et technique |
| `export_A9_*.json` | Export détaillé de l'intervention |
| `export_SQL_*.json` | Export simplifié pour intégration fournisseur |

## Responsable projet

Projet MIS / MES CLAYENS.

---

**Nom de code : A9 Tableau de bord**
