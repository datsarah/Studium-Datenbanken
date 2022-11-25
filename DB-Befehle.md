# DB-Befehle

- Beispiel Datenbank - Klausuren
    
    
    ### SQL-Befehle
    
    ```sql
    CREATE DATABASE Klausuren;
    ```
    
    ```sql
    CREATE TABLE STUDENT(
    MatrNr INTEGER,
    Name VARCHAR(45),
    PRIMARY KEY (MatrNr)
    );
    
    CREATE Table KLAUSUR(
    KNr INTEGER,
    Name Varchar(45),
    Datum DATE,
    Zeit TIME,
    PRIMARY KEY (KNr)
    );
    
    CREATE Table ANMELDUNG(
    MatrNr INTEGER,
    KNr INTEGER,
    Versuch INTEGER,
    PRIMARY KEY (MatrNr, KNr)
    CONSTRAINT Anmeldung_Klausur
    FOREIGN KEY (KNr)
    REFERENCES Klausur(KNr),
    CONSTRAINT Anmeldung_Student
    FOREIGN KEY (MatrNr)
    REFERENCES Student(MatrNr)
    );
    ```
    
    ```sql
    INSERT INTO STUDENT VALUES(1,'Meier');
    INSERT INTO STUDENT VALUES(2,'Meyer');
    INSERT INTO STUDENT VALUES(3,'Maier');
    
    INSERT INTO KLAUSUR VALUES(1,'Java 1','2018-01-14','10:00');
    INSERT INTO KLAUSUR VALUES(2,'Einführung Inf','2018-01-16', '08:00');
    INSERT INTO KLAUSUR VALUES(3,'Mathematik 1','2018-01-20','13:00');
    INSERT INTO KLAUSUR VALUES(4,'Medieninformatik','2018-01-20','08:00');
    INSERT INTO KLAUSUR VALUES(5,'Audio/Video','2018-01-28','15:30');
    
    INSERT INTO ANMELDUNG VALUES(1,2,1);
    INSERT INTO ANMELDUNG VALUES(1,4,2);
    INSERT INTO ANMELDUNG VALUES(2,1,2);
    INSERT INTO ANMELDUNG VALUES(3,3,3);
    ```
    
    

 

## DB - Allgemeine Syntax

```sql
-- am Ende einer Anweisung ;
-- Eigenschaften (z.B. Attribute) werden durch Kommata getrennt

-- Kommentar
/* 
Kommentar 
*/
```

## Operatoren und Kombinatoren

```sql
/*
=, 
>, 
≥, 
<, 
≤, 
<>(ungleich)

% -> beliebige Anzahl von Zeichen

_ -> genau ein Zeichen

AND -> WHERE name = ‘Sanders‘ AND vname = ‘Bernd‘; 
OR -> WHERE name = ‘Sanders“ OR name = ‘Krasavice‘;

LIKE -> Prüft ob der Wert mit einem Textmuster übereinstimmt
NOT LIKE -> Prüft ob der Wert nicht mit einem bestimmten Textmuster übereinstimmt

IN (Wert1, Wert2,..) -> Prüft ob der Wert in der Liste (in den Klammern) vorkommt
NOT IN (Wert1, Wert2,…) -> Prüft ob der Wert nicht in der Liste (in den Klammern) vorkommt

Wert IS NULL -> Prüft ob ein Wert nicht definiert, also leer ist
Wert IS NOT NULL -> Prüft ob ein Wert definiert ist, es liegt also ein Wert vor

… Wert BETWEEN Wert1 AND Wert2 -> ist der Wert innerhalb des Wertebereiches Wert1 und Wert2
….Wert NOT BETWEEN Wert1 AND Wert2 -> ist der Wert nicht innerhalb des Wertebereichs
*/
```

## DB - Datentypen

```sql
-- Zeichenketten
CHAR(n) -- Text mit fixer Länge n 
VARCHAR(n) -- Text mit variabler Länge, n dabei Max

-- Numerisch (Gangzzahl)
BIT, 
TINYINT, 
SMALLINT, 
INT, 
BIGINT

-- Numerisch (Nachkomma)
DECIMAL(p, s), -- p = ....., s = ....

-- Datum und Uhrzeit
DATE: -- JJJJ-MM-TT                                                           
TIME: -- hh:mm:ss
```

## DB - Datenbank und Tabellen erstellen

```sql
-- Datenbank erstellen
CREATE DATABASE [NAME]

-- Tabelle erstellen
CREATE TABLE [Tabellenname] (
	Spaltenname1[Datentyp],        
	Spaltenname2[Datentyp]                                                 
);

-- z.B.:
CREATE TABLE Verkäufer(
	Vnr INTEGER,
	Name VARCHAR(6),
	Status VARCHAR(7),
	Gehalt INTEGER,
	PRIMARY KEY (Vnr)
)

```

## DB - Datensätze hinzufügen

```sql
-- bis auf Zahlen alles in 'einfache Anführungszeichen'

-- INSERT -> alle Spalten direkt in Standartreihenfolge befüllen:
INSERT INTO[Tabellenname] VALUES(Inhalt-Spalte1,Inhalt-Spalte2,Inhalt-Spalte3);
-- z.B.
INSERT INTO Verkäufer VALUES(1001,'Udo', 'Junior'. 1500);
INSERT INTO Verkäufer VALUES(1002,'Uwe', 'Senior'. 1900);
INSERT INTO Verkäufer VALUES(1003,'Ute', 'Senior'. 2000);

-- INSERT mit Angabe der Spalten, in die die Werte eingetragen werden sollen:
-- (Reiehnfolge in der eigentlichen Tabelle hier egal)
-- nicht ausgefüllte Spalten = 
	-- entweder NULL-WERT oder
	-- Standart-Wert: z.B.: Status VARCHAR(7) DEFAULT 'Junior'
INSERT INTO Verkäufer(Vnr, Name, Status) VALUES(1004,'Ulf', 'Junior');
INSERT INTO Verkäufer(Vnr, Gehalt, Name) VALUES(1005,1300, 'Urs');
```

## DB - Datenbank/ Datensatz (teilweise) löschen

```sql
-- eine Datenbank vollständig entfernen
DROP DATABASE [Datenbankname]

-- eine Tabelle vollständig entfernen
DROP TABLE [Tabellenname]

-- Datensätze aus einer Tabelle entfernen
DELETE[Tabellenname]                                                    
-- oder                                                                               
DELETE[Tabellenname]                                                 
WHERE[Bedingung];
```

## DB - Datensätze aktualisieren

```sql
/*
Spaltenwert verändern 
(WICHTIG: Hängt man an ein UPDATE-Statement keine WHERE-Klausel an, dann wird das UPDATE auf
alle Datensätze einer Tabelle angewendet. 
Diese Funktion muss also vorsichtig eingesetzt werden, da man schnell die falschen Daten überschreiben kann.)
*/

UPDATE[Tabellenname]                            
SET[Spaltenname] = [Neuer Wert];   
                                    
-- oder
                                                                             
UPDATE[Tabellenname]                                                     
SET[Spaltenname] = [neuer Wert]                              
WHERE [Bedingung]

/*
Tabellen einer Datenbank verändern, ohne diese
komplett löschen und neu erstellen zu müssen
*/
ALTER TABLE[Tabellenname] 
ADD [Name der neuen Spalte] [Datentyp der neuen Spalte]                               

-- oder                                                                           

ALTER TABLE[Tabellenname] DROP COLUMN [Name der zu entfernenden Spalte);                                         

-- oder                                                                        

ALTER TABLE [Tabellenname] ALTER COLUMN [Spaltenname] [Neuer Datentyp];
```

## DB - Kontext festlegen

```sql
USE [Datenbankname]
```

## DB - Einschränkungen (Constraints)

```sql
- NOT NULL
- UNIQUE (keine doppelten Werte in Spalte)
- PRIMARY KEY (Primärschlüssel)
- FOREIGN KEY (Fremdschlüssel)
- CHECK (Inhalt einer Spalte vorm eintragen überprüfen)
- DEFAULT (Standartwert für Splate festlegen)
- IDENTITY (Numerischer Wert wird automatisch festgelegt)
```

## DB - einfache Daten abfragen (SELECT, FROM, WHERE)

```sql
-- Anfrage stellen:
SELECT * 
FROM[Tabellenname]

-- Ergebnis einer SELECT-Anfrage filtern
SELECT * 
FROM[Tabellenname]                                            
WHERE[Bedingung]

-- ((2) hier = Projektion) SELECT -> Was will ich wissen?
-- ((1) hier = Selektion)  FROM   -> woher kommt die Info?
-- ((1))                   WHERE  -> Bedingung

-- SELECT * = alle Datensätze 

-- SELECT distinct[Tabellenname] - Duplikate werden weggelassen
```

## DB - Daten abfragen

- ORDER BY (ASC, DESC) = sortieren
    
    ```sql
    SELECT 
    FROM
    WHERE
    ORDER BY [Spaltenname] (ASC)
    
    -- ASC -> Aufsteigend (default)
    -- DESC -> Absteigend
    
    ```
    
- GROUP BY = Gruppieren
    
    ```sql
    SELECT
    FROM
    GROUP BY Spaltenname
    ORDER BY
    ```
    
- Aggregation - Allgemein
    
    ```sql
    -- Agregation (= mehrere Zeilen gleichzeitig gearbeitet)
    SELECT Aggregatsfunktion (Spaltenname)
    FROM
    WHERE
    
    -- kann nicht in WHERE eingeschränkt werden -> dafür benötigen wir HAVING
    ```
    
- COUNT
    
    ```sql
    -- COUNT 
    
    SELECT COUNT(Spaltenname)
    FROM...
    -- Anzahl der Werte in einer Spalte zählen (auch doppelte)
    -- Null wird nicht beachtet
    
    -- COUNT(distinct Spaltenname) -> Alle unterschiedlichen Werte werden gezählt
    -- COUNT * -> Anzahl aller Zeilen in der Tabelle wird gezählt - CAVE Null
    
    ```
    
- MAX und MIN
    
    ```sql
    -- MAX = größter Wert in der Spalte
    -- MIN = kleinster Wert
    
    -- SUM(Spaltenname) -> Summe aller Werte in der Spalte 
    -- AVG(Spaltenname) -> Durchschnitt aller Werte in der Spalte 
    
    ```
    
- Spalte Umbenennen → AS
    
    ```sql
    -- AS 
    
    -- z.B.
    
    SELECT Genre,sum(Preis) AS Gesamtsumme 
    ```