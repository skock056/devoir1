# CSI2532 - Devoir 1
## Sébastien Kock, 300137051

# Partie A [60 points]: Modèles E-R

## A1 [15 points]: Relations, cardinalité et participation

### a)

![a1a](https://user-images.githubusercontent.com/71849059/109406720-76583d80-7949-11eb-96e5-cb207e2ea12a.PNG)

### b)

![a1b](https://user-images.githubusercontent.com/71849059/109406725-7ce6b500-7949-11eb-90af-a617bd4676a4.PNG)

### c)

![a1c](https://user-images.githubusercontent.com/71849059/109406728-840dc300-7949-11eb-802f-4d837d4733c6.PNG)

## A2 [30 points]: Conception du système

![a2](https://user-images.githubusercontent.com/71849059/109407287-9a1d8280-794d-11eb-8102-55756e25dcc3.PNG)

## A3 [15 points]: Algèbre relationnelle

### a)

### b)

# Partie B [60 points]: SQL

## B1 [15 points] Lecture de requêtes SQL

### a)

![b1a](https://user-images.githubusercontent.com/71849059/109406794-f1215880-7949-11eb-9a4a-6dce8eefb1ff.PNG)


### b)

![b1b](https://user-images.githubusercontent.com/71849059/109406799-f5e60c80-7949-11eb-895d-719929fdd1f4.PNG)


### c)

Pour Partie B1 c), on ne peux pas exécuter le code car le groupe créé dans le code demande pour un "id" tandis que à la ligne "GROUP BY", il manque "id" avant "name". 

L'erreur obtenue:
![b1c](https://user-images.githubusercontent.com/71849059/109406806-01d1ce80-794a-11eb-86eb-7490f6aab291.PNG)

On peut résoudre ce problème en remplacant:
```sql
GROUP BY name
```
par:
```sql
GROUP BY id, name
```

Et cela nous donne maintenant:

![b1c_answer](https://user-images.githubusercontent.com/71849059/109406918-e0bdad80-794a-11eb-8038-42ddc0100a71.PNG)


## B2 [15 points] Écriture de requêtes SQL

### a)

Pour trouver les noms des utilisateurs qui ce sont inscrits avant le 1er janvier 2021, on doit exécuter un SELECT en fonction de la variable join_date comme ceci:
```sql
SELECT name FROM users WHERE join_date < '2020-01-01';
```

Et cela nous donne:

![b2a](https://user-images.githubusercontent.com/71849059/109406926-e7e4bb80-794a-11eb-85d0-e7b702314e10.PNG)


### b)

Voici le code pour trier les licenses logicielles par nombre de licenses (en ordre décroissant):

```sql
SELECT  name,
COUNT(user_id) AS num FROM users
LEFT JOIN licenses ON users.id = licenses.user_id
GROUP BY name, id 
HAVING COUNT(*) > 0 
ORDER BY COUNT(user_id) DESC, name ASC;
```

Et cela donne:



### c)

L'ajout de données dans la table nous permet de vérifier si le code dans b) trie correctement.

Voici les données:

```sql
INSERT INTO licenses(user_id, software_name, access_code)
VALUES
 (48, 'MS Word', 'abc123'),
 (49, 'MS Word', 'def456'),
 (50, 'MS Word', 'hij789'),
 (48, 'Sketch', 'x1y2z3'),
 (51, 'Sketch', 'x2y3z4');
```

### d)

Ce bloc de code SQL met à jour la version et la date de publication de la donnée avec le nom "Sketch" dans la table "softwares".

```sql
UPDATE softwares
SET version = '51'
WHERE name = 'Sketch'
SET released_date = '2020-01-01'
WHERE name = 'Sketch';
```

## B3 [30 points] Mise à jour le schéma SQL

### a)

Le code ci-dessous ne fait que ajouter une nouvelle colonne nommée "l_version" (signifiant la version du logiciel) dans la table "licenses".

```sql
ALTER TABLE licenses
ADD COLUMN l_version varchar(100);
```

### b)

Dans psql, pour changer les clés primaires d'une table, il faut utiliser la commande "DROP CONSTRAINTS" pour supprimer les clés primaires actuelles avant d'ajouter les nouvelles clés, comme dans le code ci-dessous:

```sql
ALTER TABLE softwares
DROP CONSTRAINT softwares_pkey;

ALTER TABLE softwares
ADD PRIMARY KEY (software_name, version);
```

### c)

Ce code fait en sorte que la version du logiciel de la table "software" est dans la clé primaire de la table "licenses".

```sql
INSERT INTO licenses(user_id, software_name, access_code, version)
SELECT DISTINCT user_id, 'Sketch', '1monthfree', '52' FROM licenses
WHERE software_name = 'Sketch' AND version <> '52;
```

### d)

Dans le bloc de code SQL ci-dessous, on ajoute un "INSERT ... ON CONFLICT DO NOTHING" pour éviter la création d'une deuxième rangée dans la table. Ceci fait donc ajouter le code d'accès dans la donnée du nom de "Sketch".

```sql
INSERT INTO licenses (user_id, software_name, access_code, software_version) 
SELECT id, 'Sketch', '1monthfree', '52' FROM users ON CONFLICT DO NOTHING;
```
