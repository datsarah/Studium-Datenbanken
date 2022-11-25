# Datenbanken - Aufgabe 3

## Aufgabenstellung:
Geben Sie für die unten stehenden DB-Anfragen jeweils einen SQL-Select-Ausdruck bezogen auf die Mondial-Datenbank an.


## 1. Geben Sie alle Städte mit mehr als 9000000 Einwohnern aus.
```sql
SELECT City.Name 
FROM City
WHERE Population > 9000000;
```


## 2. Geben Sie aus, wie viele Städte mehr als 9000000 Einwohner vorweisen.
```sql
SELECT COUNT(City.Name) Stadtanzahl
FROM City
WHERE Population > 9000000;
```


## 3. Geben Sie alle Städte mit mehr als 9000000 Einwohnern und dem zugehörigen Land aus.
```sql
SELECT (City.Name) Stadtname, (Country.Name) Landname
FROM City, Country
WHERE City.Population > 9000000;
```


## 4. Geben Sie die Kürzel aller Länder aus, in denen es mindestens zwei Städte mit mehr als 5000000 Einwohner gibt.
```sql
-- SELECT MAX(City.Country) Laenderkürzel
SELECT DISTINCT(City.Country) Laenderkürzel
FROM City
WHERE City.Population > 5000000
GROUP BY City.Country
HAVING COUNT(*) >= 2;
```


## 5. Geben Sie die unterschiedlichen Namen aller Länder in alphabetischer Reihenfolge aus, in denen es mindestens zwei Städte mit mehr als 5000000 Einwohner gibt.
```sql
SELECT Country.Name
FROM City, Country
WHERE Country.Code = City.Country AND City.Population > 5000000
GROUP BY City.Country HAVING COUNT(*) >= 2
ORDER BY Name ASC;
```


## 6. Geben Sie für jedes Land in Europa in Prozent an, welchen Anteil die Landesfläche an der Fläche von Gesamteuropa hat (Ausgabe: Name des Landes, Prozent).
```sql
SELECT 
    Country.Name, (Country.Area * 100 / 
                    (SELECT Area    
                    FROM Continent 
                    WHERE Name = 'Europe')) Prozent 
FROM Country, encompasses 
WHERE Country.Code = encompasses.Country AND Continent = 'Europe';
```


## 7. Geben Sie für jedes Land die Anzahl der Städte mit mehr als 5000000 Einwohner aus (Ausgabe: Name des Landes, Anzahl).
```sql
SELECT (Country.Name) 'Name des Landes', COUNT(City.Name) Anzahl
FROM Country, City 
WHERE Country.Code = City.Country AND City.Population > 5000000 
GROUP BY Country.Name;
```


## 8. Geben Sie die Namen aller Länder aus, für die eine Stadt keine Einwohnerzahl (somit NULL) vorweist.
```sql
SELECT DISTINCT(Country.Name) 
FROM Country, City 
WHERE Country.Code = City.Country AND City.Population IS NULL;
```


## 9. Geben Sie den Ländernamen, den Namen der zugehörigen Hauptstadt und die Einwohnerzahl dieser Hauptstadt aller Länder in Amerika (Kontinent) sortiert nach den Ländernamen aus.
```sql
SELECT Country.Name, Country.Capital, City.Population
FROM Country, City, encompasses
WHERE Country.Capital = City.Name AND City.Country = Country.Code AND encompasses.Country = Country.Code AND encompasses.Continent = 'America'
ORDER BY Country.Name;
```


## 10. Geben Sie die Namen aller Länder in Europa aus, für die weder ein Fluss (GEO_RIVER) noch eine Wüste (GEO_DESERT) eingetragen sind.
```sql
SELECT Name 
FROM Country, encompasses
WHERE encompasses.Country = Country.Code AND encompasses.Continent = 'Europe' AND Country.Code NOT IN (
	SELECT Country 
    FROM geo_Desert
	union
	SELECT Country 
    FROM geo_River);
```


## 11. Geben Sie die Namen aller Länder aus, für die bei jeder Stadt dieses Landes keine Einwohnerzahl (also NULL) eingetragen ist.
```sql
SELECT Country.Name
FROM Country 
WHERE Country.Code NOT IN (
	SELECT DISTINCT Country 
    FROM City 
    WHERE City.Population IS NOT NULL
);
```


## 12. Geben Sie die Namen aller Länder aus, deren Hauptstadt weniger als 500000 Einwohner hat und für die mehr als fünf Städte in der Datenbank eingetragen sind.
```sql
SELECT Country.Name
FROM Country, City 
WHERE (Country.Capital = City.Name AND Country.Code = City.Country AND City.Population < 500000) AND Country.Code 
IN (Select Country.Code 
    From Country, City 
    WHERE Country.Code = City.Country 
    GROUP BY City.Country 
    HAVING COUNT(*) > 5);
```


## 13. Geben Sie die Namen der Länder an, die an kein Meer (sea) grenzen und deren Hauptstadt an einem Fluss liegt (located).
```sql
SELECT DISTINCT Country.Name 
FROM Country, located
WHERE Country.Code NOT IN (SELECT DISTINCT Country FROM geo_Sea) AND located.City = Country.Capital AND located.River IS NOT NULL;
```


## 14. Geben Sie alphabetisch sortiert die Namen aller Städte aus, deren Breitengrad maximal einen Grad Differenz zur geographischen Breite von Berlin hat (Berlin selbst soll sich im Ergebnis befinden).
```sql
SELECT Name 
FROM City
WHERE Latitude <= (SELECT Latitude FROM City WHERE City.Name = 'Berlin') + 1 AND Latitude >= (SELECT Latitude FROM City WHERE City.Name = 'Berlin') - 1
ORDER BY Name;
```


## 15. Geben Sie einmalig die Namen der Länder aus, von denen eine Stadt am Atlantik (Atlantic Ocean) und eine Stadt am Mittelmeer (Mediterranean Sea) liegen.
```sql
SELECT DISTINCT(Country.Name)
FROM Country, located sea1, located sea2 
WHERE
	Country.Code = sea1.Country AND
	sea1.Country = sea2.Country AND 
	sea1.City != sea2.City AND 
	sea1.Sea = 'Atlantic Ocean' AND 
	sea2.Sea = 'Mediterranean Sea';
```
