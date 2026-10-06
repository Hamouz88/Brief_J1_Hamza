# Dictionnaire des données — base Cyclo'Nord

**Nom** : Sabar Hamza **Date** : 05/10/2026  **S41 · J1 · C9**

## A. Fiche de connexion (sans mot de passe)

| Paramètre | Valeur |
|---|---|
| Hôte | `localhost` |
| Port | `5432` |
| Base | `cyclonord` |
| Compte | `lecteur_data` |
| Droits constatés | Lecture seule (`SELECT`) |
| Message d'erreur obtenu sur `UPDATE` | `SQL Error [42501]: ERROR: permission denied for table magasins` |
| Pourquoi ce compte est limité (1 phrase) | Le compte est restreint en lecture seule pour appliquer le principe de moindre privilège afin de sécuriser la base de production et éviter toute modification ou suppression involontaire. |

## B. Vue d'ensemble

| Table | Rôle (1 phrase) | Nb lignes | Nb colonnes | Clé primaire | Clés étrangères (colonne → table) |
|---|---|---|---|---|---|
| categories | Stocke les catégories et familles de produits vendues par l'entreprise. | 8 | 3 | `id_categorie` | Aucune |
| produits | Répertorie l'ensemble des articles du catalogue commercial. | 56 | 8 | `id_produit` | `id_categorie` → `categories` |
| magasins | Liste l'ensemble des points de vente physiques du réseau. | 3 | 8 | `id_magasin` | Aucune |
| employes | Contient les données relatives au personnel travaillant dans les magasins. | 18 | 10 | `id_employe` | `id_magasin` → `magasins` |
| clients | Enregistre le fichier des clients inscrits au programme ou enregistrés. | 240 | 11 | `id_client` | Aucune |
| ventes | Enregistre les entêtes de transactions réalisées en boutique. | 4 840 | 8 | `id_vente` | `id_magasin` → `magasins`, `id_employe` → `employes`, `id_client` → `clients` |
| lignes_vente | Détaille le contenu et les articles de chaque ticket de vente. | 7 968 | 6 | `id_ligne` | `id_vente` → `ventes`, `id_produit` → `produits` |

## C. Relations

| Table A | Table B | Cardinalité (A→B / B→A) | Formulation en français |
|---|---|---|---|
| categories | produits | 1,1 - 0,N | Une catégorie contient 0 à plusieurs produits, et un produit appartient obligatoirement à 1 et 1 seule catégorie. |
| produits | lignes_vente | 1,1 - 0,N | Un produit peut être présent sur 0 à plusieurs lignes de vente, et une ligne de vente concerne 1 et 1 seul produit. |
| ventes | lignes_vente | 1,1 - 1,N | Une vente se compose d'au moins 1 à plusieurs lignes de vente, et une ligne de vente appartient à 1 et 1 seule vente. |
| magasins | employes | 1,1 - 0,N | Un magasin emploie 0 à plusieurs employés, et un employé rattaché travaille dans 1 et 1 seul magasin. |
| magasins | ventes | 1,1 - 0,N | Un magasin réalise 0 à plusieurs ventes, et une vente est effectuée dans 1 et 1 seul magasin. |
| employes | ventes | 1,1 - 0,N | Un employé enregistre 0 à plusieurs ventes, et une vente est effectuée par 1 et 1 seul employé. |
| clients | ventes | 0,1 - 0,N | Un client peut passer 0 à plusieurs ventes, et une vente peut être liée à 0 ou 1 client. |

Schéma relationnel (dessin, capture DBeaver ou draw.io) :  
![Schéma Relationnel CycloNord](<Diagramme cyclonord .drawio.png>)

## D. Dictionnaire colonne par colonne

### Table : categories
| Colonne | Type | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
|---|---|---|---|---|
| `id_categorie` | integer | Identifiant unique de la catégorie (Clé primaire) | oui | non |
| `libelle` | varchar(60) | Intitulé de la catégorie de produit | oui | non |
| `description` | text | Description détaillée de la catégorie | non | non |

### Table : produits
| Colonne | Type | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
|---|---|---|---|---|
| `id_produit` | integer | Identifiant unique du produit (Clé primaire) | oui | non |
| `reference` | varchar | Référence produit interne | oui | non |
| `libelle` | varchar | Nom commercial du produit | oui | non |
| `id_categorie` | integer | Clé étrangère vers `categories` | oui | non |
| `prix_ht` | numeric | Prix de vente hors taxes | oui | non |
| `taux_tva` | numeric | Taux de TVA applicable | oui | non |
| `actif` | boolean | Produit actif au catalogue | oui | non |
| `date_creation` | date | Date de référencement | oui | non |

### Table : magasins
| Colonne | Type | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
|---|---|---|---|---|
| `id_magasin` | integer | Identifiant unique du magasin (Clé primaire) | oui | non |
| `nom` | varchar | Nom de la boutique | oui | non |
| `adresse` | varchar | Adresse physique | oui | non |
| `code_postal` | varchar | Code postal | oui | non |
| `ville` | varchar | Ville d'implantation | oui | non |
| `telephone` | varchar | Téléphone professionnel de la boutique | non | non |
| `date_ouverture` | date | Date d'ouverture | oui | non |
| `surface_m2` | integer | Surface commerciale en m² | non | non |

### Table : employes
| Colonne | Type | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
|---|---|---|---|---|
| `id_employe` | integer | Identifiant unique de l'employé (Clé primaire) | oui | oui |
| `nom` | varchar | Nom de famille | oui | oui |
| `prenom` | varchar | Prénom | oui | oui |
| `email_pro` | varchar | Email professionnel | oui | oui |
| `telephone_perso` | varchar | Numéro de téléphone personnel | non | oui |
| `date_naissance` | date | Date de naissance | oui | oui |
| `poste` | varchar | Intitulé du poste occupé | oui | non |
| `id_magasin` | integer | Clé étrangère vers `magasins` | oui | non |
| `date_embauche` | date | Date de début de contrat | oui | non |
| `date_depart` | date | Date de fin de contrat | non | non |

### Table : clients
| Colonne | Type | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
|---|---|---|---|---|
| `id_client` | integer | Identifiant unique du client (Clé primaire) | oui | oui |
| `nom` | varchar | Nom de famille | oui | oui |
| `prenom` | varchar | Prénom | oui | oui |
| `email` | varchar | Adresse email personnelle | oui | oui |
| `telephone` | varchar | Numéro de téléphone | non | oui |
| `date_naissance` | date | Date de naissance | non | oui |
| `adresse` | varchar | Adresse de résidence | non | oui |
| `code_postal` | varchar | Code postal de résidence | oui | oui |
| `ville` | varchar | Ville de résidence | oui | oui |
| `consent_newsletter` | boolean | Accord RGPD pour prospection | oui | non |
| `date_inscription` | date | Date de création du compte client | oui | non |

### Table : ventes
| Colonne | Type | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
|---|---|---|---|---|
| `id_vente` | integer | Identifiant unique de la vente (Clé primaire) | oui | non |
| `date_vente` | timestamp | Date et heure de la transaction | oui | non |
| `id_magasin` | integer | Clé étrangère vers `magasins` | oui | non |
| `id_employe` | integer | Clé étrangère vers `employes` | oui | non |
| `id_client` | integer | Clé étrangère vers `clients` | non | non |
| `statut` | varchar | État de la vente (ex. 'Validée') | oui | non |
| `mode_paiement` | varchar | Moyen de paiement utilisé | oui | non |
| `montant_total_ttc` | numeric | Montant total TTC | oui | non |

### Table : lignes_vente
| Colonne | Type | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
|---|---|---|---|---|
| `id_ligne` | integer | Identifiant unique de la ligne (Clé primaire) | oui | non |
| `id_vente` | integer | Clé étrangère vers `ventes` | oui | non |
| `id_produit` | integer | Clé étrangère vers `produits` | oui | non |
| `quantite` | integer | Nombre d'articles vendus | oui | non |
| `prix_unitaire_ht` | numeric | Prix unitaire HT appliqué | oui | non |
| `remise_pct` | numeric | Pourcentage de remise accordé | oui | non |

## E. Données à caractère personnel

| Table | Colonne | Pourquoi c'est personnel | Nécessaire pour des indicateurs de ventes ? | Si non : proposition |
|---|---|---|---|---|
| `clients` | `nom`, `prenom` | Permet d'identifier directement une personne physique. | non | Utiliser uniquement `id_client` pour regrouper les ventes. |
| `clients` | `email`, `telephone` | Coordonnées de contact directes. | non | Exclure des tables analytiques de vente. |
| `clients` | `adresse` | Géolocalisation précise d'un domicile. | non | Conserver uniquement `code_postal` et `ville` pour l'analyse géographique. |
| `clients` | `date_naissance` | Permet de connaître l'âge exact d'un individu. | non | Aggréger en tranches d'âge anonymisées (ex. 25-34 ans). |
| `employes` | `nom`, `prenom`, `email_pro` | Identifiants nominatifs du personnel. | non | Remplacer par `id_employe` dans les tableaux de bord de performance. |
| `employes` | `telephone_perso`, `date_naissance` | Informations relevant de la vie privée du salarié. | non | Supprimer du périmètre analytique. |

## F. Questions ouvertes

1. **Gestion des statuts de vente :** Quels sont les différents statuts possibles dans la colonne `ventes.statut` (ex. 'Validée', 'Annulée', 'En retour') et faut-il filtrer sur certains statuts spécifiques pour calculer le chiffre d'affaires réel ?
2. **Gestion des clients anonymes :** La colonne `id_client` dans la table `ventes` autorise-t-elle les valeurs `NULL` pour enregistrer les ventes au comptoir sans compte client ?
3. **Calcul des remises :** La colonne `lignes_vente.prix_unitaire_ht` contient-elle le prix avant ou après l'application de la remise spécifiée dans `remise_pct` ?