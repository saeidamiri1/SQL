## Contents
- [Introduction](#introduction)
- [Challenge 1](#challenge-1)
- [Challenge 2](#challenge-2)
- [Challenge 3](#challenge-3)
- [Challenge 4](#challenge-4)
- [Challenge 5](#challenge-5)
- [Challenge 6](#challenge-6)
- [Challenge 7](#challenge-7)
- [Challenge 8](#challenge-8)
- [Challenge 9](#challenge-9)
- [Challenge 10](#challenge-10)
- [Challenge 11](#challenge-11)
- [Challenge 12](#challenge-12)

## Introduction
Here we provide questions related to the topics we illustrated in [SQL at a glance]((SQL.md)). To work on questions, first download the [survey.db](https://github.com/saeidamiri1/SQL/blob/master/survey.db), this data is used in [sw](https://swcarpentry.github.io/sql-novice-survey/) to describe sqlite3, 
it is related to a historical meteorological data. This database has four tables,

-  Person: people who took readings.<br/>   
  <img src="https://github.com/saeidamiri1/SQL/blob/master/image/Person.png" width="300" height="200">

- Site: locations where readings were taken.<br/>
<img src="https://github.com/saeidamiri1/SQL/blob/master/image/Site.png" width="270" height="200"> 

- Visited: it includes when readings were taken at specific sites.<br/>
<img src="https://github.com/saeidamiri1/SQL/blob/master/image/Visited.png" width="300" height="200">

- Survey: It includes the actual readings. The field quant is short for quantitative and indicates what is being measured. Values are rad, sal, and temp referring to ‘radiation’, ‘salinity’ and ‘temperature’, respectively.<br/>
<img src="https://github.com/saeidamiri1/SQL/blob/master/image/Survey.png" width="300" height="200">

Tables in database are often related to each other using an index variable or common variables. For this database, ```survey.db```, the relation between tables are illustrated in the following figure.<br/>
<img src="https://github.com/saeidamiri1/SQL/blob/master/image/relation.png" width="300" height="200">

## Challenge 1
### Questions:
 - Load the `sutvey.db` in sqlite3.
 - How many tables are in this database?  What are the formats of column of tables?

### Solutions: 
```{sql, echo = FALSE, message = FALSE}
Leila-MacBook:~ user1$ sqlite3 survey.db
SQLite version 3.24.0 2018-06-04 14:10:15
Enter ".help" for usage hints.
sqlite>
```

orit can be  loaded from the inside of `sqlite3` 
```{sql, echo = FALSE, message = FALSE}
sqlite> .open  survey.db
```

To see the list of tables type ```.tables```, more detail of tables can be obtained using ```.schema```.  

```{sql, echo = FALSE, message = FALSE}
sqlite> .tables
Person    Site        Survey      Visited    
sqlite> .schema
CREATE TABLE Person (id text, personal text, family text);
CREATE TABLE Site (name text, lat real, long real);
CREATE TABLE Visited (id text, site text, dated text);
CREATE TABLE Survey (taken integer, person text, quant text, reading real);
```

## Challenge 2
### Questions:
 - Create two tables `NewPerson`  and `NewSite`, 
add variables (`id text`, `personal text`, `family text`) (`name text`, `lat real`, `long real`) to them, respectivley. 

### Solutions: 
```{sql, echo = FALSE, message = FALSE}
CREATE TABLE NewPerson(id text, personal text, family text);
CREATE TABLE NewSite(name text, lat real, long real);
```

## Challenge 3
### Questions:
- Select column family and personal from the table `Person`. 

### Solutions: 
```{sql, echo = FALSE, message = FALSE}
SELECT family, personal FROM Person;
```

## Challenge 4
### Questions:
- Cosider Table `Site`, add `('DR-1', -49.85, -128.57)` to this table.
- Consider Table `Site`, update `lat=-47.87` and `long=-122.40` for `name='MSK-4'`. 
- Delete `id='MSK-4'` from `Site`. 
- Create a table, call it `JustLatLong`,  and add columns `(lat text, long text)`. Then 
insert `lat`, `long` from  `Site` to `JustLatLong`. 
- Delete table `JustLatLong `that you just created. 
- Write a query to replace all ```null``` in ```Survey.person``` with the string ```unknown```.


### Solutions: 
```{sql, echo = FALSE, message = FALSE}
INSERT INTO Site VALUES('DR-1', -49.85, -128.57);
UPDATE Site SET lat=-47.87, long=-122.40 WHERE name='MSK-4';
DELETE FROM site WHERE id='MSK-4';
CREATE TABLE JustLatLong(lat text, long text);
INSERT INTO JustLatLong SELECT lat, long FROM Site;
DROP TABLE JustLatLong;
UPDATE Survey SET person = 'unknown' WHERE person IS NULL;
```

## Challenge 5
### Questions:
-  Consider the table of `Survey` and remove  the duplicates from `quant`. 
-  Consider `taken`, `person`, `quant` from  `Survey`,  order them according  ascending order `taken` , descending order `person`.

### Solutions: 
```{sql, echo = FALSE, message = FALSE}
SELECT DISTINCT quant FROM Survey;
SELECT taken, person, quant FROM Survey ORDER BY taken ASC, person DESC;
```

## Challenge 6
### Questions:
- Select values for  `site='DR-1'` in `Visited`.
- Select values `site='DR-1'` and  `dated < '1930-01-01'` in `Visited`.
- Write a query to select records that `site` start with `DR` in `Visited`.

### Solutions: 
```{sql, echo = FALSE, message = FALSE}
SELECT * FROM Visited WHERE site='DR-1';
SELECT * FROM Visited WHERE site='DR-1' AND dated < '1930-01-01';
SELECT * FROM Visited WHERE site LIKE 'DR%';
```


## Challenge 7
### Questions:
-  Calulate `1.05*taken`, `round(5*(reading-32)/9,2)` from  `Survey` for  `quant='temp'`
### Solutions: 
```{sql, echo = FALSE, message = FALSE}
SELCET 1.05*taken, round(5*(reading-32)/9,2) FROM Survey WHERE quant='temp';
```

## Challenge 8
### Questions:
- Generate `family, personal` from `Person`
- Write a query to generate union of  `Person` where `id='dyer'` and `id='roe'`.
- The following query uses  ```instr(site, '-') - 1)``` that find the index of ```-``` minus one to drop ```-```. `instr(X, Y)` clause returns the 1-based index of the first occurrence of string Y in string X, or 0 if Y does not exist in X. Write a query delete ```-``` from  `site` in `Visited`. 

### Solutions: 
```{sql, echo = FALSE, message = FALSE}
SELECT family || ', ' || personal FROM Person;
SELCET * FROM Person WHERE id='dyer' UNION SELECT * FROM Person WHERE id='roe';
SELECT DISTINCT substr(site, 1, instr(site, '-') - 1) AS MajorSite FROM Visited;
```

## Challenge 9
### Questions:
- Write a quey that show `NULL` for `dated` in the table of `Visited`.  
- Write a quey that show `Not NULL` for `dated` in the table of `Visited`.  
- Write a quey that show value of table `Survey` for  `quant = 'sal'` and  `person != 'lake'`.

### Solutions: 
```{sql, echo = FALSE, message = FALSE}
SELECT * FROM Visited WHERE dated IS NULL;
SELECT * FROM Visited WHERE dated IS NOT NULL;
SELECT * FROM Survey WHERE quant = 'sal' AND person != 'lake';
```

## Challenge 10
### Questions:
- Write a query to calculate mean of `reading` in the table `Survey`.
- Write a query to calculate mean of `reading` in the table `Survey` where `quant = 'sal'`.
- Write a query to count  `person` in the table `Survey` where  `quant = 'sal'` and  `reading <= 1.0`.
- Write a query to show `person`,  m`ax of `reading`, and  sum of reading` in the table `Survey`.
- Write a query to show `person`, `quant`, `count(reading)`, `round(avg(reading), 2)`, where `person` is not  `NULL`. 
- Rewrite previous query by group and order `person` and  `quant`.

### Solutions: 
You can retrieves the Statistical summary of variable for other variables, the following scripts generate mean and min of variable reading. 
```{sql, echo = FALSE, message = FALSE}
SELECT avg(reading) FROM Survey;
SELECT avg(reading) FROM Survey WHERE quant = 'sal';
SELECT person, count(*) FROM Survey WHERE quant = 'sal' AND reading <= 1.0;
SELECT person, max(reading), sum(reading) FROM Survey;
```

```{sql, echo = FALSE, message = FALSE}
SELECT   person, quant, count(reading), round(avg(reading), 2)
FROM     Survey
WHERE    person IS NOT NULL
SELECT   person, quant, count(reading), round(avg(reading), 2)
FROM     Survey
WHERE    person IS NOT NULL
GROUP BY person, quant
ORDER BY person, quant;
```


## Challenge 11 
### Questions:
-  Write a query to show the join of `Site` and  `Visited`
- Write a query to show the join of `Site` and  `Visited` where `Site.name = Visited.site`.
- Write a query to select `Site.lat`, `Site.long`, `Visited.dated` from `Site`  and  `Visited`  and join them where  `Site.name = Visited.site`. 
### Solutions: 

Using the ``` JOIN``` clause can creates the cross product of two tables.  
```{sql, echo = FALSE, message = FALSE}
SELECT * FROM Site JOIN Visited;
SELECT * FROM Site JOIN Visited WHERE Site.name = Visited.site;
SELECT * FROM Site JOIN Visited on Site.name = Visited.site;
SELECT Site.lat, Site.long, Visited.dated
FROM   Site JOIN Visited
ON     Site.name = Visited.site;
```

## Challenge 12
### Questions:
- Save table `Person` as CSV file, called `dataout.csv`.
- Write a query to impoert the file you just created import to sqlite3. 
- Write the database as  `survey_new.db`. 

### Solutions: 
```{sql, echo = FALSE, message = FALSE}
.header on
.mode csv
.separator ,
.once dataout.csv
SELECT * FROM Person
```

```{sql, echo = FALSE, message = FALSE}
.mode csv
.import dataout.csv tab1
```

```{sql, echo = FALSE, message = FALSE}
.save  survey_new.db;
```
