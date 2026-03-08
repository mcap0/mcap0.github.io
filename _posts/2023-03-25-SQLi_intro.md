---
title: SQL injection - intro
date: 2023-03-25 00:00:00 -500
categories: [Hacking from 0, Web Security]
tags: [sql,sql injection,database]
image_path: /assets/
--- 

# Database Vulnerabilities
Vulnerabilità web su Database:
- SQL Injection
    - Login bypass
    - Union-Based
    - Blind
        - Time based
- Logical Errors

informazioni logiche interessanti 
-
> cercare nomi tabelle, utenti, collegamenti


Database relazionali
-
CRUD (create, read, update e delete)

query per la selezione, aggiunt, delete e update, drop SQL
-
```sql
SELECT * FROM Users WHERE Username='".$_POST['username']".' AND Password='".$POST['password']."'

INSERT INTO ...
DELETE FROM ...
UPDATE Users SET Password ...
DROP TABLE Users ...

```
Operatore Union
-
unisce tabelle, solo se ritornano lo stesso numero di colonne, e sulla stessa colonna devono stare dati dello stesso tipo
```sql
SELECT u,p FROM Users UNION SELECT ...
````

## Login Bypass
```sql
' OR 1=1 -- 
-- è un commento (c'è uno spazio dopo)
```

Analisi 
-
Quanto restituito dipende dalla priorità assegnata agli operatori logici
Di solito viene interpretato prima 

> Select Where (1 AND 2) OR 3

Quindi viene restituito tutto il database
> A volte serve di più fare la injection nel campo USERNAME perchè commentando fuori il check della password viene restituito tutto il DB, e soprattutto perchè il campo password spesso viene passato come HASH.

## Union Based SQLi
serve il numero esatto di colonne, si può bruteforcare il numero di tabelle, oppure:

> ORDER BY -> specificare l'indice della colonna e bruteforcare il numero di colonne usando questa query, il numero 31 funziona, il 32 no, la tabella ha 31 colonne

## Recuperare nomi Tabelle e Colonne

alcuni DB hanno una tabella particolare Database, information_schema:

al suo interno troviamo tables e columns
```SQL
SELECT column_name FROM information_schema.columns WHERE table_name = "Users"
```

Schema enumeration(?) GIYF Google is your Frient

Group Concat
-
```sql
SELECT title,post FROM posts WHERE id =  1 and 1=0 UNION SELECT 1,group_concat(username, ':', password) FROM Users -- 
```

unisce risultati in più righe, in un'unica riga

## Blind SQLi
Se non è possibile vedere il risultato di una UNION...


Il sito si comporta in modi diversi a seconda di cosa viene ritornato dal DB -> ORACOLO is the win key
> Un form di login potrebbe ritornare Login Successful se viene ritornata anche solo una tupla


operatore LIKE 
serve a elaborare risultati con regex
```sql
AND password LIKE 'a%'
```
se ritorna qualcosa la pass inizia con a -> si usa questo oracolo per decifrare la password

wildcards con LIKE
-
- % -> 0+ caratteri
- _ -> 1 carattere
> per rendere case-sensitive usi LIKE BINARY

substring
-
```sql
AND (SELECT ... AND substring(password, 1, 1)= 'a') 
substring(password, 1, 2) = 'aa')
 ```

## Time-Based

Se non cambia comportamento in nessun modo, bhe, lì ci azzicchi una sleep(2).
a quel punto 

```sql
SELECT sleep(2)
```
a questo punto diventa uguale alle blind

### Remediation
Gli escape possono tralasciare cose

invece: le STORED PROCEDURES sono impossibili da exploitare

Object Relational Mapping

le nostre entità di database diventano classi e vengono sanitizzati automaticamente

## Falle Logiche

errori semantici (confronti tra stringhe e interi)
Interpretazioni errate
> controllare se il flow di esecuzione è corretto

ad esempio:
> per resettare una password usa uno script che richiede un username e un token valido -> crei un token valido e lo usi per un username a caso

mysql payload:
"1111 UNION (SELECT GROUP_CONCAT(table_name))

UNION (SELECT GROUP_CONCAT(column_name) FROM information_schema.columns WHERE table_schema = DATABASE()) --")


# PAyloads
Union Based SQL Injections

Finally, it's time to retrieve the flag. But where is it? We need to find out by looking at what tables and columns are in the database.

By googling at the version number that you retrieved before, you should know that the DBMS that the backend is using is MySQL. MySQL provides a useful schema to retrieve database metadata, called INFORMATION_SCHEMA (link).

Two of the tables of this schema are very useful for what we need to do:

The tables table (Sorry for the repetition..) that contains a list of all tables accessible by the current user. Its most useful columns are:
table_name: the name of the table
table_schema: the schema that contains the table
The columns table that contains a list of all the columns of all the tables accessible by the current user. Its most useful columns are:
column_name: The name of the column
table_name: The name of the table that contains the column
table_schema: The name of the schema that contains the table that contains the column
Using the following query, you can retrieve all the tables that are accessible by the current user

```mysql 
SELECT table_name FROM information_schema.tables
```
Because there are a lot of ""system"" tables, that don't contain any useful information, you may want to exclude them by querying only for tables that are in the current schema. In order to retrieve the current schema, you can use the function DATABASE(). So the above query becomes:

```mysql
SELECT table_name FROM information_schema.tables WHERE table_schema = DATABASE()
```
And for retrieving a list of all columns of a particular table:

```mysql
SELECT column_name FROM information_schema.columns WHERE table_name = '*name_of_the_table*'
```
You can also retrieve all columns-tables with the folliwn query:

```mysql
SELECT table_name,column_name from information_schema.columns WHERE table_schema=DATABASE()
```
Now you have everything that you need to retrive the flag; You only need to adapt the queries. The only problem you can encounter is the closing ', that will break your sintax. You have two options:

You comment it out, using the MySQL comment syntax -- (note that it needs a space character at the end)
```mysql
' UNION SELECT first_column,second_column,3,4,5,6 FROM table WHERE somecolumn='something' -- 
```
You can add an AND clause to the where that is always true, for example:
>' UNION SELECT first_column,second_column,3,4,5,6 FROM table WHERE somecolumn='something' and 1='1
In this way, the append quote will close the 1='1 , and the query will be syntactically correct.

