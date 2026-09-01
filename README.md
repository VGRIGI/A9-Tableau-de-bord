# A9 Tableau de bord

Application web de suivi des modifications apportées aux paramètres du process d'injection pendant une production.

Le projet **A9 Tableau de bord** a été conçu dans le cadre du programme MIS de CLAYENS afin de fournir aux régleurs un outil simple, visuel et utilisable depuis un navigateur web. L'application permet d'enregistrer les ajustements réalisés sur une presse à injecter lors de la résolution d'un problème qualité, puis de générer un fichier JSON destiné à l'alimentation d'une data plateforme.

## Objectifs

- Identifier l'ordre de fabrication concerné.
- Identifier le régleur ayant réalisé l'intervention.
- Sélectionner le type de rebut constaté.
- Enregistrer uniquement les paramètres réellement modifiés.
- Conserver les valeurs avant et après modification.
- Gérer les paramètres comportant plusieurs paliers, zones ou thermocouples.
- Vérifier le respect du remplissage dynamique avant l'enregistrement définitif.
- Générer un fichier JSON exploitable par la data plateforme.
- Afficher un historique des interventions réalisées pendant la session.

## Technologies

L'application est volontairement autonome et légère :

- HTML5
- CSS3
- JavaScript natif
- Aucun framework
- Aucune dépendance à installer
- Compatible avec un hébergement statique, notamment GitHub Pages

Le fichier principal de l'application est :

```text
index.html
```

## Utilisation

### 1. Identification

Au démarrage, l'utilisateur renseigne :

- le numéro d'OF ;
- le nom et le prénom du régleur.

Le numéro d'OF est contrôlé selon le format suivant :

```text
FF26201122/20
```

Règle appliquée :

```text
2 lettres + 8 chiffres + / + 2 chiffres
```

### 2. Déclaration du rebut

Le régleur sélectionne un type de rebut parmi la liste prédéfinie :

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

Un champ commentaire général est disponible.

Lorsque le type de rebut **Autre** est sélectionné, une fenêtre demande obligatoirement une description complémentaire.

### 3. Saisie des modifications process

Les paramètres sont organisés en trois onglets principaux :

- ⚙️ Process injection
- 🌡️ Températures
- 🏭 Moule

#### Process injection

Sous-onglets :

- 🧪 Dosage
- ⚡ Dynamique
- ⏱️ Maintien

##### Dosage

| Paramètre | Type de saisie | Unité(s) |
|---|---|---|
| Course de dosage | Avant / Après | mm ou cm³ |
| Vitesse de rotation | Paliers ajoutables | Tr/min ou m/s |
| Contre-pression | Avant / Après | bar ou bar spécifique |
| Décompression | Avant / Après | mm ou cm³/s |
| Temps de refroidissement | Avant / Après | secondes |

La **Course de dosage** est affichée en première position.

##### Dynamique

| Paramètre | Type de saisie | Unité(s) |
|---|---|---|
| Vitesse | Paliers ajoutables | mm/s ou cm³/s |
| Course | Paliers ajoutables | mm ou cm³ |
| Temps d'injection | Avant / Après | secondes |
| Point de commutation en course | Avant / Après | mm ou cm³ |
| Point de commutation en pression | Avant / Après | bar ou bar spécifique |

##### Maintien

| Paramètre | Type de saisie | Unité(s) |
|---|---|---|
| Pression | Paliers ajoutables | bar ou bar spécifique |
| Temps | Paliers ajoutables | secondes |
| Matelas | Avant / Après | mm ou cm³ |

#### Températures

Sous-onglets :

- 🔥 Fourreau
- ♨️ Bloc chaud

##### Température fourreau

- Buse
- Zone 1 à Zone 6 par défaut
- Possibilité d'ajouter des zones supplémentaires
- Valeurs Avant / Après
- Unité : °C

##### Température bloc chaud

- Zone 1 à Zone 16 par défaut
- Possibilité d'ajouter des zones supplémentaires
- Valeurs Avant / Après
- Unité : °C

#### Moule

| Paramètre | Type de saisie | Unité(s) |
|---|---|---|
| Force de verrouillage | Avant / Après | Tonne ou KN |
| Température partie fixe | Thermos ajoutables | °C |
| Température partie mobile | Thermos ajoutables | °C |

Chaque partie du moule possède un thermo par défaut et permet d'en ajouter d'autres.

## Règle Avant / Après

Une valeur **Après** ne peut pas être saisie tant que la valeur **Avant** correspondante n'a pas été renseignée.

Comportement :

1. Le champ Après est désactivé par défaut.
2. La saisie de la valeur Avant déverrouille le champ Après.
3. L'effacement de la valeur Avant vide et reverrouille automatiquement le champ Après.
4. Cette règle s'applique aux paramètres simples, aux paliers, aux zones de température et aux thermos.

## Avertissement sur les paramètres majeurs

L'application affiche le message suivant juste au-dessus des onglets :

> ⚠️ **Attention :** les paramètres majeurs suivants : **temps d'injection, température matière, outillages, pression de maintien et tolérances matelas** ne peuvent faire l'objet d'aucune modification autre que dans les tolérances définies sans validation interne, qualité, fabrication et externe client. **(Si exigence contractuelle)**

Cet avertissement est informatif et ne bloque pas la saisie.

## Contrôle du remplissage dynamique

Lorsque le régleur clique sur **Enregistrer**, une fenêtre obligatoire apparaît avec la question :

```text
Remplissage dynamique respecté ?
```

Trois réponses sont possibles :

- C : Conforme
- NC : Non conforme
- NA : Non applicable

La fenêtre affiche également le message :

> Si le remplissage dynamique n'est pas respecté, alerter le service qualité et le chef d'équipe pour décision.

L'intervention ne peut pas être enregistrée tant qu'une réponse n'a pas été sélectionnée.

La réponse est :

- conservée dans l'historique de session ;
- ajoutée au fichier JSON exporté dans le champ `RemplissageDynamique`.

## Export JSON

À la validation d'une intervention, l'application génère un fichier JSON contenant :

- le numéro d'OF ;
- la réponse au contrôle du remplissage dynamique ;
- les paramètres modifiés ;
- les unités sélectionnées ;
- les valeurs Avant et Après ;
- les paliers, zones et thermos concernés.

Exemple :

```json
{
  "NumeroOF": "FF26201122/20",
  "RemplissageDynamique": "C",
  "ParametresModifies": {
    "Course de dosage": {
      "Avant": "35",
      "Après": "32",
      "unite": "mm"
    },
    "Pression (Maintien)": {
      "Palier 1": {
        "Avant": "450",
        "Après": "470"
      },
      "unite": "bar spécifique"
    }
  }
}
```

Seuls les paramètres renseignés sont inclus dans le fichier.

## Nom du fichier exporté

Le fichier est nommé automatiquement selon le principe suivant :

```text
export_NUMERO_OF_HORODATAGE.json
```

Exemple :

```text
export_FF26201122_20_2026-09-01T13-45-12-000Z.json
```

## Choix du dossier d'enregistrement

Lorsque le navigateur prend en charge la **File System Access API**, l'application demande de sélectionner un dossier lors du premier enregistrement.

Le dossier sélectionné est utilisé pour les enregistrements suivants pendant la session du navigateur.

Un message indique le dossier actif.

Si l'accès direct au système de fichiers n'est pas pris en charge ou si la sélection est annulée, l'application utilise le téléchargement classique du navigateur.

### Navigateur recommandé

- Microsoft Edge
- Google Chrome

La sélection directe d'un dossier peut ne pas être disponible dans tous les navigateurs ou dans certains contextes d'hébergement.

## Historique de session

Après l'enregistrement, l'historique affiche notamment :

- le numéro d'OF ;
- le type de rebut ;
- le nom du régleur ;
- la réponse au remplissage dynamique ;
- la liste des paramètres modifiés ;
- la date et l'heure ;
- le nom du fichier JSON généré.

L'historique est stocké uniquement dans la mémoire de la page. Il est perdu lorsque la page est actualisée ou fermée.

## Hébergement GitHub Pages

Le projet peut être hébergé gratuitement sur GitHub Pages.

Dépôt du projet :

```text
https://github.com/VGRIGI/A9-Tableau-de-bord
```

Adresse GitHub Pages attendue :

```text
https://vgrigi.github.io/A9-Tableau-de-bord/
```

### Publication

Dans les paramètres du dépôt GitHub :

1. Ouvrir **Settings**.
2. Sélectionner **Pages**.
3. Choisir **Deploy from a branch**.
4. Sélectionner la branche `main`.
5. Sélectionner le dossier `/ (root)`.
6. Enregistrer.

À chaque remplacement du fichier `index.html` sur la branche `main`, GitHub Pages republie automatiquement l'application.

## Mise à jour du projet sur GitHub

Pour déployer une nouvelle version depuis l'interface web GitHub :

1. Télécharger le nouveau fichier généré.
2. Le renommer en `index.html` si nécessaire.
3. Ouvrir le dépôt GitHub.
4. Ouvrir le fichier `index.html` existant.
5. Utiliser **Edit this file** ou **Upload files**.
6. Remplacer le contenu ou le fichier.
7. Créer un commit sur la branche `main`.
8. Actualiser GitHub Pages avec `Ctrl + F5` si l'ancienne version reste en cache.

## Charte graphique

La version actuelle conserve la disposition historique de l'application et utilise la palette suivante :

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

Des pictogrammes sont présents uniquement dans les onglets principaux et les sous-onglets.

## Limites actuelles

- L'application ne possède pas de base de données.
- L'historique n'est pas conservé après fermeture ou actualisation de la page.
- L'identité du régleur est saisie manuellement.
- Aucun lien avec SIDAQ n'est actif dans cette version.
- Aucun lien avec Cyclades n'est actif dans cette version.
- L'envoi vers la data plateforme repose actuellement sur un fichier JSON et non sur une API ou un broker MQTT.
- Le dépôt et le site GitHub Pages sont publics si aucune authentification supplémentaire n'est mise en place.

## Évolutions possibles

Les évolutions suivantes ne font pas partie de la version actuelle mais peuvent être étudiées ultérieurement :

- envoi automatique du JSON vers une API ;
- publication MQTT vers la data plateforme ;
- stockage permanent des interventions ;
- identification du régleur par badge ou compte utilisateur ;
- récupération automatique de l'OF ;
- connexion au SIDAQ ;
- connexion à Cyclades ;
- exploitation Power BI ;
- intégration native dans la GED ;
- gestion des droits et validations qualité.

## Structure du dépôt

```text
A9-Tableau-de-bord/
├── index.html
└── README.md
```

## Statut

Prototype fonctionnel destiné à :

- valider l'ergonomie avec les régleurs ;
- cadrer les données process à enregistrer ;
- valider le format JSON avec les équipes data ;
- préparer l'intégration dans la GED et la data plateforme.

## Responsable projet

Projet MIS / MES CLAYENS.

---

**Nom de code : A9 Tableau de bord**
