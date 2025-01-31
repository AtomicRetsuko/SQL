# SQL location de ski
This is my SQL Repository
## En premier, créer la abse de donnée :

CREATE DATABASE location_ski;

## Ensuite,
### Requetes pour créer la base de donnée : 

CREATE TABLE clients (
    noCli INT PRIMARY KEY,
    nom VARCHAR(50) NOT NULL,
    prenom VARCHAR(50) NOT NULL,
    adresse VARCHAR(100) NOT NULL,
    cpo VARCHAR(10) NOT NULL,  -- Keeping it VARCHAR for flexibility
    ville VARCHAR(50) NOT NULL
);

CREATE TABLE fiches (
    noFic INT PRIMARY KEY,
    noCli INT NOT NULL,
    dateCrea DATE NOT NULL,
    datePaiement DATE NULL,
    etat ENUM('SO', 'EC', 'RE') NOT NULL,  -- SO: Soldé, EC: En cours, RE: Retard
    FOREIGN KEY (noCli) REFERENCES clients(noCli) ON DELETE CASCADE
);

CREATE TABLE tarifs (
    codeTarif VARCHAR(5) PRIMARY KEY,
    libelle VARCHAR(50) NOT NULL,
    prixJour DECIMAL(10,2) NOT NULL CHECK (prixJour > 0)
);

CREATE TABLE gammes (
    codeGam VARCHAR(5) PRIMARY KEY,
    libelle VARCHAR(50) NOT NULL
);

CREATE TABLE categories (
    codeCate VARCHAR(5) PRIMARY KEY,
    libelle VARCHAR(50) NOT NULL
);

CREATE TABLE grilleTarifs (
    codeGam VARCHAR(5) NOT NULL,
    codeCate VARCHAR(5) NOT NULL,
    codeTarif VARCHAR(5) NOT NULL,
    PRIMARY KEY (codeGam, codeCate, codeTarif),
    FOREIGN KEY (codeGam) REFERENCES gammes(codeGam) ON DELETE CASCADE,
    FOREIGN KEY (codeCate) REFERENCES categories(codeCate) ON DELETE CASCADE,
    FOREIGN KEY (codeTarif) REFERENCES tarifs(codeTarif) ON DELETE CASCADE
);

CREATE TABLE articles (
    refart VARCHAR(10) PRIMARY KEY,
    designation VARCHAR(100) NOT NULL,
    codeGam VARCHAR(5) NOT NULL,
    codeCate VARCHAR(5) NOT NULL,
    FOREIGN KEY (codeGam) REFERENCES gammes(codeGam) ON DELETE CASCADE,
    FOREIGN KEY (codeCate) REFERENCES categories(codeCate) ON DELETE CASCADE
);

CREATE TABLE lignesFic (
    noFic INT NOT NULL,
    noLig INT NOT NULL,
    refart VARCHAR(10) NOT NULL,
    depart DATE NOT NULL,
    retour DATE NULL,
    PRIMARY KEY (noFic, noLig),
    FOREIGN KEY (noFic) REFERENCES fiches(noFic) ON DELETE CASCADE,
    FOREIGN KEY (refart) REFERENCES articles(refart) ON DELETE CASCADE
);

Ensuite insérer la Data :

USE location_ski;
INSERT INTO clients (noCli, nom, prenom, adresse, cpo, ville) VALUES 
    (1, 'Albert', 'Anatole', 'Rue des accacias', '61000', 'Amiens'),
    (2, 'Bernard', 'Barnabé', 'Rue du bar', '1000', 'Bourg en Bresse'),
    (3, 'Dupond', 'Camille', 'Rue Crébillon', '44000', 'Nantes'),
    (4, 'Desmoulin', 'Daniel', 'Rue descendante', '21000', 'Dijon'),
     (5, 'Ernest', 'Etienne', 'Rue de l’échaffaud', '42000', 'Saint Étienne'),
    (6, 'Ferdinand', 'François', 'Rue de la convention', '44100', 'Nantes'),
    (9, 'Dupond', 'Jean', 'Rue des mimosas', '75018', 'Paris'),
    (14, 'Boutaud', 'Sabine', 'Rue des platanes', '75002', 'Paris');

INSERT INTO fiches (noFic, noCli, dateCrea, datePaiement, etat) VALUES 
    (1001, 14,  DATE_SUB(NOW(),INTERVAL  15 DAY), DATE_SUB(NOW(),INTERVAL  13 DAY),'SO' ),
    (1002, 4,  DATE_SUB(NOW(),INTERVAL  13 DAY), NULL, 'EC'),
    (1003, 1,  DATE_SUB(NOW(),INTERVAL  12 DAY), DATE_SUB(NOW(),INTERVAL  10 DAY),'SO'),
    (1004, 6,  DATE_SUB(NOW(),INTERVAL  11 DAY), NULL, 'EC'),
    (1005, 3,  DATE_SUB(NOW(),INTERVAL  10 DAY), NULL, 'EC'),
    (1006, 9,  DATE_SUB(NOW(),INTERVAL  10 DAY),NULL ,'RE'),
    (1007, 1,  DATE_SUB(NOW(),INTERVAL  3 DAY), NULL, 'EC'),
    (1008, 2,  DATE_SUB(NOW(),INTERVAL  0 DAY), NULL, 'EC');

INSERT INTO tarifs (codeTarif, libelle, prixJour) VALUES
    ('T1', 'Base', 10),
    ('T2', 'Chocolat', 15),
    ('T3', 'Bronze', 20),
    ('T4', 'Argent', 30),
    ('T5', 'Or', 50),
    ('T6', 'Platine', 90);

INSERT INTO gammes (codeGam, libelle) VALUES
    ('PR', 'Matériel Professionnel'),
    ('HG', 'Haut de gamme'),
    ('MG', 'Moyenne gamme'),
    ('EG', 'Entrée de gamme');

INSERT INTO categories (codeCate, libelle) VALUES
    ('MONO', 'Monoski'),
    ('SURF', 'Surf'),
    ('PA', 'Patinette'),
    ('FOA', 'Ski de fond alternatif'),
    ('FOP', 'Ski de fond patineur'),
    ('SA', 'Ski alpin');

INSERT INTO grilleTarifs (codeGam, codeCate, codeTarif) VALUES
    ('EG', 'MONO', 'T1'),
    ('MG', 'MONO', 'T2'),
    ('EG', 'SURF', 'T1'),
    ('MG', 'SURF', 'T2'),
    ('HG', 'SURF', 'T3'),
    ('PR', 'SURF', 'T5'),
    ('EG', 'PA', 'T1'),
    ('MG', 'PA', 'T2'),
    ('EG', 'FOA', 'T1'),
    ('MG', 'FOA', 'T2'),
    ('HG', 'FOA', 'T4'),
    ('PR', 'FOA', 'T6'),
    ('EG', 'FOP', 'T2'),
    ('MG', 'FOP', 'T3'),
    ('HG', 'FOP', 'T4'),
    ('PR', 'FOP', 'T6'),
    ('EG', 'SA', 'T1'),
    ('MG', 'SA', 'T2'),
    ('HG', 'SA', 'T4'),
    ('PR', 'SA', 'T6');

INSERT INTO articles (refart, designation, codeGam, codeCate) VALUES 
    ('F01', 'Fischer Cruiser', 'EG', 'FOA'),
    ('F02', 'Fischer Cruiser', 'EG', 'FOA'),
    ('F03', 'Fischer Cruiser', 'EG', 'FOA'),
    ('F04', 'Fischer Cruiser', 'EG', 'FOA'),
    ('F05', 'Fischer Cruiser', 'EG', 'FOA'),
    ('F10', 'Fischer Sporty Crown', 'MG', 'FOA'),
    ('F20', 'Fischer RCS Classic GOLD', 'PR', 'FOA'),
    ('F21', 'Fischer RCS Classic GOLD', 'PR', 'FOA'),
    ('F22', 'Fischer RCS Classic GOLD', 'PR', 'FOA'),
    ('F23', 'Fischer RCS Classic GOLD', 'PR', 'FOA'),
    ('F50', 'Fischer SOSSkating VASA', 'HG', 'FOP'),
    ('F60', 'Fischer RCS CARBOLITE Skating', 'PR', 'FOP'),
    ('F61', 'Fischer RCS CARBOLITE Skating', 'PR', 'FOP'),
    ('F62', 'Fischer RCS CARBOLITE Skating', 'PR', 'FOP'),
    ('F63', 'Fischer RCS CARBOLITE Skating', 'PR', 'FOP'),
    ('F64', 'Fischer RCS CARBOLITE Skating', 'PR', 'FOP'),
    ('P01', 'Décathlon Allegre junior 150', 'EG', 'PA'),
    ('P10', 'Fischer mini ski patinette', 'MG', 'PA'),
    ('P11', 'Fischer mini ski patinette', 'MG', 'PA'),
    ('S01', 'Décathlon Apparition', 'EG', 'SURF'),
    ('S02', 'Décathlon Apparition', 'EG', 'SURF'),
    ('S03', 'Décathlon Apparition', 'EG', 'SURF'),
    ('A01', 'Salomon 24X+Z12', 'EG', 'SA'),
    ('A02', 'Salomon 24X+Z12', 'EG', 'SA'),
    ('A03', 'Salomon 24X+Z12', 'EG', 'SA'),
    ('A04', 'Salomon 24X+Z12', 'EG', 'SA'),
    ('A05', 'Salomon 24X+Z12', 'EG', 'SA'),
    ('A10', 'Salomon Pro Link Equipe 4S', 'PR', 'SA'),
    ('A11', 'Salomon Pro Link Equipe 4S', 'PR', 'SA'),
    ('A21', 'Salomon 3V RACE JR+L10', 'PR', 'SA');

INSERT INTO lignesFic (noFic, noLig,  refart, depart, retour) VALUES 
    (1001, 1, 'F05', DATE_SUB(NOW(),INTERVAL  15 DAY), DATE_SUB(NOW(),INTERVAL  13 DAY)),
    (1001, 2, 'F50', DATE_SUB(NOW(),INTERVAL  15 DAY), DATE_SUB(NOW(),INTERVAL  14 DAY)),
    (1001, 3, 'F60', DATE_SUB(NOW(),INTERVAL  13 DAY), DATE_SUB(NOW(),INTERVAL  13 DAY)),
    (1002, 1, 'A03', DATE_SUB(NOW(),INTERVAL  13 DAY), DATE_SUB(NOW(),INTERVAL  9 DAY)),
    (1002, 2, 'A04', DATE_SUB(NOW(),INTERVAL  12 DAY), DATE_SUB(NOW(),INTERVAL  7 DAY)),
    (1002, 3, 'S03', DATE_SUB(NOW(),INTERVAL  8 DAY), NULL),
    (1003, 1, 'F50', DATE_SUB(NOW(),INTERVAL  12 DAY), DATE_SUB(NOW(),INTERVAL  10 DAY)),
    (1003, 2, 'F05', DATE_SUB(NOW(),INTERVAL  12 DAY), DATE_SUB(NOW(),INTERVAL  10 DAY)),
    (1004, 1, 'P01', DATE_SUB(NOW(),INTERVAL  6 DAY), NULL),
    (1005, 1, 'F05', DATE_SUB(NOW(),INTERVAL  9 DAY), DATE_SUB(NOW(),INTERVAL  5 DAY)),
    (1005, 2, 'F10', DATE_SUB(NOW(),INTERVAL  4 DAY), NULL),
    (1006, 1, 'S01', DATE_SUB(NOW(),INTERVAL  10 DAY), DATE_SUB(NOW(),INTERVAL  9 DAY)),
    (1006, 2, 'S02', DATE_SUB(NOW(),INTERVAL  10 DAY), DATE_SUB(NOW(),INTERVAL  9 DAY)),
    (1006, 3, 'S03', DATE_SUB(NOW(),INTERVAL  10 DAY), DATE_SUB(NOW(),INTERVAL  9 DAY)),
    (1007, 1, 'F50', DATE_SUB(NOW(),INTERVAL  3 DAY), DATE_SUB(NOW(),INTERVAL  2 DAY)),
    (1007, 3, 'F60', DATE_SUB(NOW(),INTERVAL  1 DAY), NULL),
    (1007, 2, 'F05', DATE_SUB(NOW(),INTERVAL  3 DAY), NULL),
    (1007, 4, 'S02', DATE_SUB(NOW(),INTERVAL  0 DAY), NULL),
    (1008, 1, 'S01', DATE_SUB(NOW(),INTERVAL  0 DAY), NULL);


  Question 1) :
   Liste des clients (toutes les informations) dont le nom commence par un D

   SELECT * 
FROM clients 
WHERE nom LIKE 'D%';

Question 2) : 

 Nom et prénom de tous les clients

 SELECT nom, prenom 
FROM clients;

Question 3) :

 Liste des fiches (n°, état) pour les clients (nom, prénom) qui habitent en Loire Atlantique (44)

 SELECT f.noFic, f.etat, c.nom, c.prenom
FROM fiches f
JOIN clients c ON f.noCli = c.noCli
WHERE c.cpo LIKE '44%';

5️⃣ Prix journalier moyen de location par gamme

SELECT g.libelle AS gamme, AVG(t.prixJour) AS prix_moyen_journalier
FROM grilleTarifs gt
JOIN gammes g ON gt.codeGam = g.codeGam
JOIN tarifs t ON gt.codeTarif = t.codeTarif
GROUP BY g.libelle;

Question 6) :

6️⃣ Détail de la fiche n°1002 avec le total

SELECT 
    lf.noFic, 
    lf.noLig, 
    lf.refart, 
    a.designation, 
    g.libelle AS gamme, 
    c.libelle AS categorie, 
    t.prixJour, 
    DATEDIFF(NOW(), lf.depart) AS jours_loc, 
    (DATEDIFF(NOW(), lf.depart) * t.prixJour) AS total_par_article
FROM lignesFic lf
JOIN articles a ON lf.refart = a.refart
JOIN gammes g ON a.codeGam = g.codeGam
JOIN categories c ON a.codeCate = c.codeCate
JOIN grilleTarifs gt ON a.codeGam = gt.codeGam AND a.codeCate = gt.codeCate
JOIN tarifs t ON gt.codeTarif = t.codeTarif
WHERE lf.noFic = 1002;

Question 7) :

7️⃣ Grille des tarifs

SELECT 
    g.libelle AS gamme, 
    c.libelle AS categorie, 
    t.libelle AS tarif, 
    t.prixJour AS prix_journalier
FROM grilleTarifs gt
JOIN gammes g ON gt.codeGam = g.codeGam
JOIN categories c ON gt.codeCate = c.codeCate
JOIN tarifs t ON gt.codeTarif = t.codeTarif
ORDER BY g.libelle, c.libelle, t.prixJour;

Question 8) :
8️⃣ Liste des locations de la catégorie SURF

SELECT 
    lf.noFic, 
    lf.noLig, 
    lf.refart, 
    a.designation, 
    g.libelle AS gamme, 
    c.libelle AS categorie, 
    lf.depart, 
    lf.retour
FROM lignesFic lf
JOIN articles a ON lf.refart = a.refart
JOIN gammes g ON a.codeGam = g.codeGam
JOIN categories c ON a.codeCate = c.codeCate
WHERE c.libelle = 'Surf'
ORDER BY lf.noFic, lf.noLig;

Question 9 ) :
9️⃣ Calcul du nombre moyen d’articles loués par fiche de location

SELECT 
    AVG(article_count) AS moyenne_articles_par_fiche
FROM (
    SELECT 
        noFic, 
        COUNT(*) AS article_count
    FROM lignesFic
    GROUP BY noFic
) AS subquery;

Question 10) :
10 - Calcul du nombre de fiches de location établies pour les catégories de location Ski alpin, Surf et Patinette

SELECT 
    c.libelle AS categorie,
    COUNT(DISTINCT lf.noFic) AS nombre_de_fiches
FROM lignesFic lf
JOIN articles a ON lf.refart = a.refart
JOIN categories c ON a.codeCate = c.codeCate
WHERE c.libelle IN ('Ski alpin', 'Surf', 'Patinette')
GROUP BY c.libelle;

Question 11 ):
11 Calcul du montant moyen des fiches de location

SELECT 
    AVG(montant_total) AS montant_moyen
FROM (
    SELECT 
        lf.noFic, 
        SUM(t.prixJour * DATEDIFF(IFNULL(lf.retour, NOW()), lf.depart)) AS montant_total
    FROM lignesFic lf
    JOIN articles a ON lf.refart = a.refart
    JOIN tarifs t ON a.codeGam = t.codeGam AND a.codeCate = t.codeCate
    GROUP BY lf.noFic
) AS subquery;


### fin


