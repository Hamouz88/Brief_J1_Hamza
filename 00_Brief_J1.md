# Brief · Lundi 5 octobre 2026 — Explorer la base Cyclo'Nord (J1/5)

| | |
|---|---|
| **Formation** | Data Analyst 2026-2027 · HDF / Arras |
| **Semaine** | S41 (05/10 – 09/10) · P4 · **Phase 1 · Ajuster** |
| **Thème de la semaine** | Explorer la base et les données des indicateurs |
| **Techno** | PostgreSQL · SQL · DBeaver |
| **Formateur** | Ayoub · présentiel · 7 h |
| **Livrable de la semaine (V 09/10)** | Requêtes commentées + tableau indicateur → données |

---

## Compétences visées

Référentiel Simplon — Data Analyst (niveau 6) :

| Code | Compétence | Ce qu'on travaille aujourd'hui |
|---|---|---|
| **C6** | Extraire des données depuis une source structurée de manière sécurisée | Lister les contraintes techniques d'accès à la base (connexion, compte en lecture, RGPD) |
| **C9** | Structurer une zone de données intermédiaire (modèles conceptuels et physiques, MERISE) | Lire un modèle existant : tables, clés, relations ; amorcer un dictionnaire des données |
| **C12** | Développer des requêtes de type SQL pour calculer les indicateurs | Définir l'objectif, les tables et les colonnes nécessaires à une requête (préalable aux requêtes de mardi/mercredi) |
| **C2** | Concevoir un produit data en qualifiant les données nécessaires au calcul des indicateurs | Décrire la source (nom, type, modalités d'accès) ; préparer le repérage données disponibles / manquantes (J4) |

Niveau de la phase : **Phase 1 · Ajuster** — l'apprenant travaille sur une base et un modèle fournis, avec un exemple guidé, et ajuste à partir de l'existant.
Correspondance ancien référentiel WCS (onglet *Vue semaines*, encore non réaligné) : C1.3 et C1.4, niveau 1 · Imiter.

---

## Contexte — fil rouge Cyclo'Nord

Cyclo'Nord est un réseau de trois magasins de vélos et d'un atelier de réparation dans les Hauts-de-France (Arras, Lens, Douai). La semaine dernière, vous avez contrôlé le fichier de ventes exporté en CSV avec Python. Problème : ce fichier est une extraction partielle et vieillissante. La directrice veut désormais que l'équipe data travaille **directement sur la base de production**, en lecture seule, pour préparer un suivi mensuel de l'activité (indicateurs calculés en SQL la semaine prochaine).

Avant d'écrire la moindre requête, il faut **comprendre comment la base est construite** : quelles tables, quelles clés, quelles relations, quelles données personnelles.

---

## Objectifs de la journée

À la fin de la journée, chaque apprenant·e sait :

1. expliquer ce qu'est un SGBD, situer PostgreSQL et distinguer base / schéma / table / colonne / ligne ;
2. se connecter à une base PostgreSQL avec DBeaver via un compte en lecture seule, et dire pourquoi ce compte est limité ;
3. lire le modèle d'une base existante : identifier tables, clés primaires, clés étrangères et cardinalités ;
4. documenter ce modèle sous forme d'un début de dictionnaire des données ;
5. repérer les colonnes contenant des données personnelles.

---

## Ressources fournies

- **Accès base** : hôte, port, nom de base et identifiants du compte `lecteur_data` (lecture seule) — transmis sur Simplonline, à ne jamais versionner.
- **DBeaver Community** installé lors de la semaine de prise en main (sinon : voir fiche d'installation S38).
- **Schéma physique de la base Cyclo'Nord** (image + script DDL commenté).
- **Exemple guidé** : lecture complète de la table `magasins` et de sa relation avec `ventes` (modèle de fiche à reproduire).
- **Modèle de dictionnaire des données** (tableau Markdown vide à compléter).
- Rappel des 7 tables : `magasins`, `employes`, `clients`, `produits`, `categories`, `ventes`, `lignes_vente`.

---

## Déroulé

### Matin — Comprendre et se connecter (9h–12h30)

| Durée | Séquence | Modalité |
|---|---|---|
| 45 min | Apport : SGBD, SGBDR, PostgreSQL, vocabulaire (base, schéma, table, clé, type), lecture seule et principe de moindre privilège | Apport formateur + Q/R |
| 30 min | Démo : connexion DBeaver, navigation dans l'arborescence, aperçu des données, onglet ER Diagram | Démonstration |
| 1 h 15 | Atelier 1 : connexion à la base Cyclo'Nord, exploration libre des tables, premières observations (volumétrie, colonnes, types) | Individuel |
| 30 min | Mise en commun : qu'avez-vous vu ? qu'est-ce qui vous surprend ? | Collectif |

### Après-midi — Lire et documenter le modèle (13h30–17h)

| Durée | Séquence | Modalité |
|---|---|---|
| 30 min | Apport : clé primaire, clé étrangère, cardinalités (1-N, N-N via table de liaison), lecture d'un diagramme entité-association | Apport formateur |
| 30 min | Exemple guidé : fiche de lecture de `magasins` ↔ `ventes` (reproduite pas à pas) | Collectif |
| 1 h 30 | Atelier 2 : fiche de lecture des 5 autres tables + dictionnaire des données + repérage des données personnelles | Binômes |
| 30 min | Restitution : 3 binômes présentent une table ; vérification croisée avec le DDL | Collectif |
| 15 min | Dépôt du livrable du jour, annonce de la suite (mardi : contraintes, jointures simples, accès RGPD) | — |

---

## Travail à réaliser

### Atelier 1 — Se connecter et explorer

1. Créer la connexion PostgreSQL dans DBeaver avec les identifiants `lecteur_data`.
2. Tester la connexion. Tenter un `UPDATE` sur une table : constater le refus, noter le message d'erreur et l'expliquer en une phrase.
3. Pour chaque table : nombre de lignes approximatif (onglet *Statistiques* ou aperçu), nombre de colonnes, colonne qui semble identifier chaque ligne.
4. Noter 3 questions que vous vous posez sur les données (ex. « que signifie `statut` dans `ventes` ? »).

### Atelier 2 — Lire et documenter le modèle

5. Reproduire, pour chaque table, la fiche de lecture de l'exemple guidé : clé primaire, clés étrangères, table(s) liée(s), cardinalité.
6. Dessiner (papier, DBeaver ou draw.io) le schéma relationnel complet : 7 tables et leurs liens.
7. Compléter le dictionnaire des données : table · colonne · type · description · obligatoire ? · donnée personnelle ?
8. Lister les colonnes contenant des données à caractère personnel (clients, employés) et proposer, pour chacune, si elle est nécessaire au calcul d'indicateurs de ventes.

---

## Livrable du jour

Un fichier `J1_lecture_modele_cyclonord.md` déposé sur Simplonline avant 17h, contenant :

- la fiche de connexion (hôte / port / base / compte — **sans mot de passe**) et la contrainte d'accès constatée ;
- le schéma relationnel (image ou texte) ;
- le dictionnaire des données des 7 tables ;
- la liste des colonnes à caractère personnel ;
- les 3 questions ouvertes.

Ce livrable alimente directement le livrable de vendredi (tableau indicateur → données) et le registre des traitements qui sera rédigé en phase 2.

---

## Critères de réussite

Issus des critères d'évaluation du référentiel, adaptés à la phase 1 :

- [ ] La connexion fonctionne avec le compte en lecture seule ; la limitation d'accès est expliquée (C6 — *contraintes techniques des sources listées*).
- [ ] Chaque table est décrite avec sa clé primaire et ses clés étrangères, sans erreur par rapport au DDL (C9 — *modélisation explicitée sans erreur d'interprétation*).
- [ ] Le dictionnaire des données couvre les 7 tables et toutes leurs colonnes (C9 — *documentation des schémas formalisée dans un dictionnaire*).
- [ ] Les colonnes à caractère personnel sont identifiées et leur nécessité questionnée (C6 / C9 — *respect du RGPD*).
- [ ] La source est décrite : nom, type, modalités d'accès (C2 — *sources décrites avec nom, type, modalités d'accès*).
- [ ] Aucun identifiant ni mot de passe n'apparaît dans le livrable (C6 — *standards de sécurité*).

---

## Différenciation

- **Déjà à l'aise avec SQL** : écrire, pour chaque relation identifiée, une requête de vérification d'intégrité (ex. lignes de `ventes` sans magasin correspondant) — sans jointure complexe, on les voit mardi. Préparer un mini-exposé de 5 min sur la différence entre `VARCHAR` et `TEXT` en PostgreSQL.
- **Découverte complète** : commencer par les trois tables de l'exemple guidé élargi (`magasins`, `produits`, `ventes`), puis étendre. L'objectif minimal est le dictionnaire de ces trois tables, correct et complet.
- Les binômes de l'après-midi mélangent les deux profils.

---

## Ce qui n'est pas attendu aujourd'hui

- Pas de requête SQL au-delà de la lecture des tables (vient mardi et mercredi).
- Pas de modification du modèle (phase 1 : on lit l'existant, on ne le reconçoit pas).
- Pas de calcul d'indicateur (semaine S42).

---

## Veille (C3 — fil continu)

Question ouverte à ramener mardi, avec la source consultée et une phrase sur sa fiabilité (auteur, date, recoupement) : *« Pourquoi une entreprise donne-t-elle à son équipe data un compte en lecture seule plutôt qu'un accès complet ? »*
