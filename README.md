---
author: Lektion 2
date: MMMM dd, YYYY
paging: "%d / %d"
---

# Lektion 2

Hej och välkommen!

## Agenda

1. Frågor, quiz och repetition
2. Introduktion till PostgreSQL
3. Genomgång av
   1. SQL
   2. Queries
   3. Tabeller
   4. Rader & kolumner
4. Övning med handledning
5. Quiz frågor

---

# Quiz frågor

- Vad står 'SQL' för?
- Vilken typ av databas är PostgreSQL?
- Vilket språk använder PostgreSQL?
- Vad är SQL databaser bra på?
- Vad kan jämföras med rader och kolumner i C#?
- Vad kallas `NOT NULL` om det finns i en tabell?
- Vad gör `SELECT` i SQL?
- Vilket Docker kommando används för att ladda ned en image?
- Vad är en Docker container?

---

# Introduktion till PostgreSQL

PostgreSQL är en modern relationsdatabas. Den är väl dokumenterad, open source, snabb och har mycket funktionalitet.

- SQL som språk
- Stödjer endast strukturerad data
- Hanterar komplexa strukturer genom relationer
- Anpassad för effektiv sökning med strukturerad data

Vi installerar och hanterar PostgreSQL genom Docker.

<https://www.postgresql.org/>

---

# Få igenom en PostgreSQL databas

## Steg 1: Starta Docker Desktop och Docker Engine

Öppna upp Docker Desktop, och, om Docker Engine inte är igång, starta den.

## Steg 2: Öppna terminalen

Öppna upp valfri terminal separat från Docker Desktop.

## Steg 3: Ladda ned PostgreSQL och starta en container

Kör följande i terminalen:

1. `docker pull postgres`
2. `docker run -p 5432:5432 -e POSTGRES_PASSWORD=password --name my-postgres -d postgres`

Ange valfritt lösenord, och valfritt namn på container. 'my-postgres' används som namn ovan.

---

# Öppna PostgreSQL klient för att kunna skriva SQL

## Steg 1: Starta container

Starta en PostgreSQL container, om den är stoppad. Om du har kört `docker run` så är detta gjort redan.

## Steg 2: Gå in i container

Kör följande i terminalen: `docker exec -it my-postgres bash`

(byt ut 'my-postgres' mot ditt container namn)

## Steg 3: Anslut till databas genom psql

Kör följande i terminalen: `psql -U postgres`

(ange sedan lösenord om det efterfrågas)

---

# Interna databaser

PostgreSQL är en databas och körs som program i Docker. Inom PostgreSQL finns det också "databaser". Dessa "interna" databaser är till för att organisera olika projekt. Varje projekt har en egen intern databas.

Ni kan skapa en databas för den individuella uppgiften och en för grupp-uppgiften. Ni kan även, om ni vill, skapa en databas per övning, eller per vecka med övningar.

Skapa med: `CREATE DATABASE <name>;`, exempelvis:

- `CREATE DATABASE todo;`
- `CREATE DATABASE personal-finance;`

Gå in i databas med: `\c <name>`, exempelvis:

- `\c todo`
- `\c personal-finance`

Lista upp databaser med: `\l`

---

# Tabeller

En tabell är en strukturerad samling med data.

- Struktur måste bestämmas (kolumner, datatyper och constraints)
- Strukturen i sig kallas 'schema'
- Innehåller rader och kolumner
  - Varje rad är som ett objekt
  - Varje kolumn är som ett fält/variabel för ett objekt

Skapa tabeller med: `CREATE TABLE <name> (columns)`, exempelvis:

```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(50) NOT NULL,
  email VARCHAR(100) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# Skapa tabell endast om den inte redan finns

```sql
CREATE TABLE IF NOT EXISTS users (
  name TEXT,
  email VARCHAR(100)
);
```

# Radera tabell och data

```sql
DROP TABLE users;
```

# Se vilka tabeller som finns i databas

`\dt`

---

# Kolumner och datatyper

- boolean
- integer
- decimal
- text
- varchar
- timestamp
- date

Läs mer: <https://www.postgresql.org/docs/current/datatype.html>

---

# CRUD med SQL

CRUD (Create, Read, Update, Delete) har matchande nyckelord i SQL.

- CREATE - INSERT
- READ - SELECT
- UPDATE - UPDATE
- DELETE - DELETE

---

# Lägg in data i tabell (CREATE)

Syntax:

```sql
INSERT INTO table (columns) VALUES (values1), (values2);
```

Exempel:

```sql
INSERT INTO products (id, title, price) VALUES (3, 'Candy', 5.3);

INSERT INTO users (name, email)
VALUES ('Ironman', 'tony@stark.com'),
       ('Superman', 'clark@kent.com'),
       ('Batman', 'bruce@wayne.com');
```

---

# Hämta/sök data från tabell (READ)

Syntax:

```sql
SELECT columns FROM table WHERE condition;
```

Exempel:

```sql
-- Hämta alla rader och kolumner
SELECT * FROM users;

-- Hämta alla rader men endast 'name' kolumnen
SELECT name FROM users;

-- Hämta alla rader som heter 'Ironman', och endast email
SELECT email FROM users WHERE name = 'Ironman';

-- Räkna rader
SELECT COUNT(*) FROM users WHERE created_at > '2023-01-01';
```

---

# Uppdatera/ändra på data i tabell (UPDATE)

Syntax:

```sql
UPDATE table SET column = value WHERE condition;
```

Exempel:

```sql
-- Uppdatera alla rader
UPDATE users SET status = 'active';

-- Uppdatera specifik kolumn för en användare baserat på namn
UPDATE users SET email = 'tony@stark.com' WHERE name = 'Ironman';

-- Uppdatera baserat på datum
UPDATE users 
SET membership_level = 'premium'
WHERE created_at > '2023-01-01';
```

---

# Radera data från tabell (DELETE)

Syntax:

```sql
DELETE FROM table WHERE condition;
```

Exempel:

```sql
-- Ta bort alla rader 
DELETE FROM users;

-- Ta bort specifik användare baserat på namn
DELETE FROM users WHERE name = 'Ironman';

-- Ta bort användare baserat på flera villkor
DELETE FROM users 
WHERE status = 'inactive' 
AND last_login < '2023-01-01';
```

---

# Villkor i queries

```sql
-- Flera villkor
SELECT * FROM products 
WHERE price > 100 
AND stock_quantity <= 50;

-- LIKE och wildcards
SELECT name, email 
FROM users 
WHERE email LIKE '%@gmail.com' 
OR email LIKE '%@hotmail.com';

-- IN för flera möjliga värden
SELECT * FROM orders 
WHERE status IN ('pending', 'processing', 'shipped')
AND total_amount > 1000;

-- BETWEEN för intervaller
SELECT * FROM sales 
WHERE sale_date BETWEEN '2023-01-01' AND '2023-12-31'
AND amount BETWEEN 1000 AND 5000;
```

---

# Fler villkor i queries

```sql
-- NULL-hantering
SELECT * FROM customers 
WHERE phone_number IS NOT NULL 
AND last_order_date IS NULL;

-- Kombinera datum och text
SELECT * FROM employees 
WHERE hire_date < '2023-01-01'
AND (department = 'Sales' OR department = 'Marketing')
AND salary >= 50000;

-- Datum-beräkningar
SELECT * FROM log_entries 
WHERE created_at >= DATE_SUB(NOW(), INTERVAL 24 HOUR)
AND severity IN ('ERROR', 'CRITICAL')
AND message NOT LIKE '%timeout%';

-- Kombinera flera villkor med parenteser
SELECT * FROM orders 
WHERE (status = 'completed' AND total_amount > 1000)
OR (status = 'pending' AND created_at < DATE_SUB(NOW(), INTERVAL 7 DAY))
ORDER BY created_at DESC;
```

---

# Quiz frågor

- Vad står 'SQL' för?
- Vad är en tabell?
- Vad är en rad?
- Vad kan jämföras med rader och kolumner i C#?
- Vad gör `SELECT` i SQL?
- Vad gör `INSERT` i SQL?
- Vad betyder `SELECT * FROM table;`?
- Vilket nyckelord används för att lägga till villkor i queries?
- Vad är kommandot för att skapa en ny tabell?
- Vilket nyckelord används för att slå ihop två villkor?
- Vad är skillnaden mellan `LIKE` och `=` i WHERE-villkor?
- Vad gör `LIMIT` i en SELECT?
- Vad gör `DISTINCT` i en SELECT?
- Vad är en "primary key" och varför är den viktig?
- Vad är skillnaden mellan `NULL` och en tom sträng ('')?
