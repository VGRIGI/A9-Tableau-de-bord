# A9 Tableau de bord

Prototype web pour la saisie et le suivi des modifications de paramètres process apportées par les régleurs en cours de production injection (projet MIS — CLAYENS).

## Objectif

Permettre à un régleur de tracer, de façon simple et visuelle, les modifications de paramètres process réalisées pour résoudre une problématique qualité (bavure, retassure, etc.), et d'exporter automatiquement ces données au format JSON pour alimentation de la data plateforme.

## Fonctionnalités

- **Écran 1 — Identification** : saisie du n° d'OF (format verrouillé `FF26201122/20`) et du nom/prénom du régleur.
- **Écran 2 — Déclaration d'intervention** :
  - Sélection du type de rebut (liste prédéfinie + option "Autre" avec commentaire obligatoire en pop-up)
  - Champ commentaire libre optionnel
  - Saisie des paramètres modifiés, organisés en 3 groupes d'onglets :
    - **Process injection** : Dosage (vitesse de rotation, contre-pression, décompression, temps de refroidissement), Dynamique (vitesse, point de commutation en course/pression), Maintien (pression, temps — paliers multiples)
    - **Températures** : zones fourreau (Buse, Zone 1-6, extensible), zones bloc chaud (16 zones par défaut, extensible)
    - **Moule** : force de verrouillage (Tonne/KN), température et régulation outillage
  - Gestion des unités multiples par paramètre (ex. bar / bar hydraulique, Tr/Min / M/s, mm/s / cm³/s)
  - Paliers multiples ajoutables/supprimables pour les paramètres qui le nécessitent
- **Export automatique** : à l'enregistrement, génération d'un fichier JSON (n° OF + paramètres modifiés) directement écrit dans un dossier choisi par l'utilisateur (via File System Access API), avec repli sur téléchargement classique si non supporté par le navigateur.
- **Historique de session** : liste des interventions enregistrées, consultable et ré-exportable.

## Utilisation

Ouvrir `index.html` directement dans un navigateur (Chrome ou Edge recommandé pour le support de l'écriture directe de fichier). Aucune installation ni dépendance requise.

## Structure du fichier JSON exporté

```json
{
  "NumeroOF": "FF26201122/20",
  "ParametresModifies": {
    "Vitesse de rotation": {
      "unite": "Tr/Min",
      "Palier 1": { "Avant": "120", "Après": "135" }
    },
    "Zone 3": {
      "unite": "°C",
      "Avant": "210",
      "Après": "215"
    }
  }
}
```

## Limitations connues (prototype)

- Les données sont conservées en mémoire de session uniquement (pas de base de données).
- L'écriture directe de fichier nécessite un navigateur compatible avec la File System Access API (Chrome/Edge). Pour un déploiement en production avec écriture automatique et silencieuse (ex. vers `C:\`), une politique de navigateur (GPO) doit être configurée par l'IT.
- Pas encore de connexion au SIDAQ ni à Cyclades (écarté du périmètre pour cette version).

## Statut

Prototype d'essai destiné à valider l'ergonomie et le format d'export avec les équipes qualité/production avant développement définitif (intégration GED, connexion data plateforme SISE/DATALYO).
