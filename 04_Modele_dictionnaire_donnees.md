# Dictionnaire des données — base Cyclo'Nord

**Nom** : Briquet Loris  **Date** : 05/10/2026  **S41 · J1 · C9**

## A. Fiche de connexion (sans mot de passe)

| Paramètre                                | Valeur                           |
| ---------------------------------------- | -------------------------------- |
| Hôte                                     | localhost                        |
| Port                                     | 5432                             |
| Base                                     | cyclonord                        |
| Compte                                   | lecteur_data                     |
| Droits constatés                         | READ                             |
| Message d'erreur obtenu sur `UPDATE`     | error 42501                      |
| Pourquoi ce compte est limité (1 phrase) | Permission de lecture uniquement |

## B. Vue d'ensemble

| Table        | Rôle (1 phrase)                                                          | Nb lignes | Nb colonnes | Clé primaire | Clés étrangères (colonne → table) |
| ------------ | ------------------------------------------------------------------------ | --------- | ----------- | ------------ | --------------------------------- |
| categories   | liste les catégories                                                     |           |             |              |                                   |
| produits     | pareil                                                                   |           |             |              |                                   |
| magasins     | pareil                                                                   |           |             |              |                                   |
| employes     | pareil                                                                   |           |             |              |                                   |
| clients      | pareil                                                                   |           |             |              |                                   |
| ventes       | pareil                                                                   |           |             |              |                                   |
| lignes_vente | liste les ventes individuelles de produits par vente (passage en caisse) |           |             |              |                                   |

## C. Relations

| Table A    | Table B      | Cardinalité (A→B / B→A) | Formulation en français                                                                                |
| ---------- | ------------ | ----------------------- | ------------------------------------------------------------------------------------------------------ |
| categories | produits     | 1, n / n, 1             | Chaque catégorie comporte plusieurs produits mais chaque produit n'appartient qu'à une seule catégorie |
| produits   | lignes_vente | 1, 1 / 1, 1             |                                                                                                        |
| ventes     | lignes_vente | 1, n / n, 1             |                                                                                                        |
| magasins   | employes     | 1, n / n, 1             |                                                                                                        |
| magasins   | ventes       | 1, n / n, 1             |                                                                                                        |
| employes   | ventes       | 1, n / n, 1             |                                                                                                        |
| clients    | ventes       | 1, n / n, 1             |                                                                                                        |

Schéma relationnel (dessin, capture DBeaver ou draw.io) : *à joindre*.

## D. Dictionnaire colonne par colonne

Une ligne par colonne. Dupliquer le bloc pour chaque table.

### Table : clients

| Colonne            | Type          | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
| :----------------- | :------------ | :---------- | :-------------------- | :--------------------------- |
| id_client          | serial        | id unique   | oui                   |                              |
| nom                | varchar (60)  |             | oui                   | oui                          |
| prenom             | varchar (60)  |             | oui                   | oui                          |
| email              | varchar (120) |             |                       | oui                          |
| telephone          | varchar (14)  |             |                       | oui                          |
| date_naissance     | date          |             |                       | oui                          |
| adresse            | varchar (120) |             |                       | oui                          |
| code_postal        | bpchar (5)    |             |                       | oui                          |
| ville              | varchar (60)  |             |                       | oui                          |
| consent_newsletter | bool          |             | oui                   |                              |
| date_inscription   | date          |             | oui                   |                              |

---

### Table : employes

| Colonne         | Type           | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
| :-------------- | :------------- | :---------- | :-------------------- | :--------------------------- |
| id_employe      | serial / int   | id unique   | oui                   |                              |
| nom             | varchar / text |             | oui                   | oui                          |
| prenom          | varchar / text |             | oui                   | oui                          |
| email_pro       | varchar / text |             |                       | oui                          |
| telephone_perso | varchar / text |             |                       | oui                          |
| date_naissance  | date           |             |                       | oui                          |
| poste           | varchar / text |             | oui                   |                              |
| id_magasin      | int            |             | oui                   |                              |
| date_embauche   | date           |             | oui                   |                              |
| date_depart     | date           |             |                       |                              |

---

### Table : magasins

| Colonne        | Type           | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
| :------------- | :------------- | :---------- | :-------------------- | :--------------------------- |
| id_magasin     | serial / int   | id unique   | oui                   |                              |
| nom            | varchar / text |             | oui                   |                              |
| adresse        | varchar / text |             |                       |                              |
| code_postal    | varchar / text |             | oui                   |                              |
| ville          | varchar / text |             | oui                   |                              |
| telephone      | varchar / text |             |                       |                              |
| date_ouverture | date           |             | oui                   |                              |
| surface_m2     | int / numeric  |             |                       |                              |

---

### Table : ventes

| Colonne           | Type             | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
| :---------------- | :--------------- | :---------- | :-------------------- | :--------------------------- |
| id_vente          | serial / int     | id unique   | oui                   |                              |
| date_vente        | date / timestamp |             | oui                   |                              |
| id_magasin        | int              |             | oui                   |                              |
| id_employe        | int              |             | oui                   |                              |
| id_client         | int              |             |                       |                              |
| statut            | varchar / text   |             | oui                   |                              |
| mode_paiement     | varchar / text   |             | oui                   |                              |
| montant_total_ttc | numeric          |             | oui                   |                              |

---

### Table : lignes_vente

| Colonne          | Type         | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
| :--------------- | :----------- | :---------- | :-------------------- | :--------------------------- |
| id_ligne         | serial       | id unique   | oui                   |                              |
| id_vente         | int          |             | oui                   |                              |
| id_produit       | int          |             | oui                   |                              |
| quantite         | int          |             | oui                   |                              |
| prix_unitaire_ht | numeric(9,2) |             | oui                   |                              |
| remise_pct       | numeric(4,1) |             | oui                   |                              |

---

### Table : produits

| Colonne       | Type             | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
| :------------ | :--------------- | :---------- | :-------------------- | :--------------------------- |
| id_produit    | serial / int     | id unique   | oui                   |                              |
| reference     | varchar / text   |             | oui                   |                              |
| libelle       | varchar / text   |             | oui                   |                              |
| id_categorie  | int              |             | oui                   |                              |
| prix_ht       | numeric          |             | oui                   |                              |
| taux_tva      | numeric          |             | oui                   |                              |
| actif         | boolean          |             | oui                   |                              |
| date_creation | date / timestamp |             | oui                   |                              |

---

### Table : categories

| Colonne      | Type           | Description | Obligatoire (oui/non) | Donnée personnelle (oui/non) |
| :----------- | :------------- | :---------- | :-------------------- | :--------------------------- |
| id_categorie | serial / int   | id unique   | oui                   |                              |
| libelle      | varchar / text |             | oui                   |                              |
| description  | text           |             |                       |                              |


## E. Données à caractère personnel

| Colonne        | Tables             | Pourquoi c'est personnel                                                 | Nécessaire pour des indicateurs de ventes ? | Si non : proposition                           |
| -------------- | ------------------ | ------------------------------------------------------------------------ | ------------------------------------------- | ---------------------------------------------- |
| nom            | clients + employés |                                                                          | non                                         | On peut s'en séparer                           |
| prenom         | clients + employés |                                                                          | non                                         | On peut s'en séparer                           |
| email          | clients + employés |                                                                          | non                                         | On garde, sert à contacter le client           |
| tel            | clients + employés |                                                                          | non                                         | On garde, sert à contacter le client           |
| date naissance | clients + employés |                                                                          | oui : stats selon l'âge                     |                                                |
| adresse        | clients            |                                                                          | oui : stats selon le lieu                   |                                                |
| code postal    | clients            |                                                                          | oui : stats selon le lieu                   |                                                |
| ville          | clients            |                                                                          | oui : stats selon le lieu                   |                                                |
| id             | clients + employés | Sert à identifier une personne, on peut retrouver des infos grâce à l'id | non                                         | Est indispensable à la structure de ces tables |

## F. Questions ouvertes

- Pourquoi il y a des ventes avec un id_client NULL ?
- A quoi correspondent les produits non-actifs ?
- Pourquoi certains clients sont enregistrés mais ont des infos manquantes (e.g mail/tel/date de naissance)
