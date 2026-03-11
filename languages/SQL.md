
# Concepts

## SQL vs. NoSQL

| SQL            | NoSQL                                                                              |
| -------------- | ---------------------------------------------------------------------------------- |
| relational     | non-relational                                                                     |
| defined schema | dynamic schema                                                                     |
| table-based    | variety of different storage methods, such as document, key-value, graph and more. |
# SQL Databases

## SQLite

### Characteristics

- *DOES NOT* enforce type checking, you could write an integer into a TEXT column

### PRAGMA

#### table_info
> `PRAGMA table_info(your_table_name)` ist ein SQLite-Kommando (genauer: ein "Pragma"), das dir die *Struktur einer Tabelle* zurückgibt 
> 
> `table_info` is a keyword here
> 
> [SQLite Introspection](https://wiki.tcl-lang.org/revision/SQLite+introspection?V=6). 
- es zeigt für jede Spalte einer Tabelle:
	- Spalten-ID (`cid`)
	- Spaltenname (`name`)
	- Datentyp (`type`)
	- NOT NULL-Constraint (`notnull`: 0=nein, 1=ja)
	- Standardwert (`dflt_value`)
	- Primary Key-Zugehörigkeit (`pk`: 0=nein, 1=ja)[](https://raw.githubusercontent.com/Onelinerhub/onelinerhub/main/sqlite/how-do-i-get-the-column-names-of-a-sqlite-database.md)
- example:
```
PRAGMA table_info(sounds);

cid | name         | type    | notnull | dflt_value | pk
0   | id           | TEXT    | 1       | NULL       | 1
1   | description  | TEXT    | 1       | NULL       | 0
2   | duration     | REAL    | 1       | NULL       | 0
3   | favourite    | BOOLEAN | 0       | 0          | 0
4   | last_updated | TIMESTAMP| 0      | NULL       | 0
```
- another way to do it in modern SQLite:
	- instead of doing `PRAGMA` 
```python
def column_exists(self, table_name, column_name):
    cursor = self.database.execute("""
        SELECT COUNT(*) FROM pragma_table_info(?)
        WHERE name = ?
    """, (table_name, column_name))
    return cursor.fetchone()[0] > 0
```

#### user_version

> Used to check the db version of the user.

```python
# Version setzen
cursor.execute("PRAGMA user_version = 2")
# Version lesen
cursor.execute("PRAGMA user_version")
version = cursor.fetchone()[0]
print(f"Datenbank-Version: {version}")
```
# TODO TO SORT

## `COUNT(*)`

> Can be used to check if a colum exists in a given table
```python
def column_exists(self, table_name, column_name):
    cursor = self.database.execute("""
        SELECT COUNT(*) FROM pragma_table_info(?)
        WHERE name = ?
    """, (table_name, column_name))
    return cursor.fetchone()[0] > 0
```

## Type operators?

- `PRIMARY KEY`
- `NOT NULL`
- `UNIQUE`
- `DEFUALT <default value>`

## constraints?

```sql
CREATE TABLE IF NOT EXISTS favorites (
    id INTEGER PRIMARY KEY AUTOINCREMENT,  
    title TEXT NOT NULL CHECK(LENGTH(title) > 0),
    url TEXT UNIQUE NOT NULL,
    duration INTEGER CHECK(duration > 0),
    added_date DATE DEFAULT (datetime('now')),
    category TEXT DEFAULT 'uncategorized',
    is_favorite BOOLEAN DEFAULT 0
)
```
# Commands

```sql
# These operations NEED commit():
cursor.execute('UPDATE favorites SET title="Forest" WHERE id=1')
cursor.execute('DELETE FROM favorites WHERE id=3')
conn.commit()  # Save these changes permanently
```
## TABLE

### CREATE

> Creating a table
> 
> 🔥  `IF NOT EXISTS` checks only if this table exists, so if it already exsits and you add here some columns it *will not* update. Only new tables will be affected, for changes you need `ALTER TABLE`.
```sql
-- structure
CREATE TABLE <CONDITION> table_name (col1, col2, ...)

-- example
CREATE TABLE IF NOT EXISTS favorites (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL
	added_date DATE DEFAULT CURRENT_DATE
	last_updated TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
```

#### Basic Types

```sql
INTEGER
TEXT
BOOLEAN -- 🔥 NOTICE SQLITE WILL STORE THAT AS 0 & 1
DATE
TIMESTAMP
```

### ALTER

> Modify table structure.
```sql
-- rename TABLE
ALTER TABLE sounds
RENAME TO fav_sounds

-- 🔥 every COLUMN modification requires a separate ALTER TABLE statement
-- rename COLUMN
ALTER TABLE sounds
RENAME COLUMN name TO title

-- add COLUMN
ALTER TABLE sounds 
ADD COLUMN was_listened BOOLEAN DEFAULT 0

-- remove COLUMN
ALTER TABLE sounds
DROP COLUMN is_favourite
```

> Most basic migration
```python
def migrate_schema(self):
    """Führt alle notwendigen Schema-Migrationen durch"""
    migrations = [
        ("sounds", "was_listened", "BOOLEAN DEFAULT 0"),
        ("sounds", "play_count", "INTEGER DEFAULT 0"),
        ("sounds", "last_played", "TIMESTAMP"),
    ]
    
    for table, column, definition in migrations:
        if not self.column_exists(table, column):
            self.database.execute(f"""
                ALTER TABLE {table} 
                ADD COLUMN {column} {definition}
            """)
    
    self.database.commit()
```


### DROP

> Delete table. TODO

## INSERT

```sql
INSERT INTO favorites (title) VALUES ("Birdsong")
INSERT INTO favorites (title, url) VALUES (?, ?)

?
INSERT OR IGNORE INTO tags (name) VALUES (?)
```

## UPDATE

> general structure
```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...   
WHERE condition
```
## SELECT

> Basics
```sql
-- select all columns from table favourites
SELECT * FROM favourites

-- select age and name from table friends
SELECT name, age FROM favourites

-- conditional select
SELECT title FROM favourites WHERE id=1

-- get count of rows in favourite table
SELECT COUNT(*) FROM favourites
```

```sql
SELECT 
	id, 
	name, 
	priority, 
    strftime('%Y-%m-%d', added_date) as added,
	CASE WHEN executed IS NULL THEN 'active' ELSE 'eliminated' END as status
FROM peopletokill
ORDER BY priority DESC, added_date
```

### JOIN

```sql
SELECT f.title, f.url 
            FROM favorites f
            JOIN favorite_tags ft ON f.id = ft.favorite_id
            JOIN tags t ON ft.tag_id = t.id
            WHERE t.name = ?
```
## Advanced

### TRIGGER

> General SQL TRIGGER structure:
```sql
CREATE TRIGGER trigger_name
{ BEFORE | AFTER } { INSERT | UPDATE | DELETE }
ON table_name
[ FOR EACH ROW ]  -- Row-level triggers
[ WHEN condition ]
BEGIN
    -- trigger body with access to NEW and OLD, for example:
    UPDATE categories SET last_updated = CURRENT_TIMESTAMP 
    WHERE id = NEW.id;
END;
```
- So they can trigger on table insertions, updates, delets.
- Trigger before and after the actual change
- TODO: for each row?
- TODO: when?
- Then they have body between `BEGIN` & `END` desciribing what should happen in the trigger.

### Aggregates

> Five functions: `COUNT`, `MAX`, `MIN`, `AVG`, `SUM`) 
> 
> The ***fundamental building blocks*** of data analysis in SQL. 
> 
> They've been *in every SQL implementation* since the language was created in the 1970s!

```sql
-- Works in SQLite, PostgreSQL, MySQL, SQL Server, Oracle, etc.
SELECT 
    COUNT(*) as total_rows,
    MAX(price) as highest_price,
    MIN(price) as lowest_price,
    AVG(price) as average_price,
    SUM(quantity) as total_quantity
FROM products;
```

> They are often used with `GROUP BY`:
```SQL
-- This assumes that the fields in category table are not unique and you perform grouping operations on them, so like COUNT(*) will in this case return number of repetitions of a certain item type 
SELECT 
    category,
    COUNT(*) as items_in_category,
    AVG(price) as avg_price_by_category
FROM products
GROUP BY category;
```

> Usecase example:
```sql
-- Example with multiple rows per category
CREATE TABLE sound_effects (
    id INTEGER PRIMARY KEY,
    category TEXT,
    duration_seconds INTEGER,
    filename TEXT
);
-- Multiple sounds in Nature category
INSERT INTO sound_effects VALUES 
(1, 'Nature', 30, 'rain.wav'),
(2, 'Nature', 45, 'wind.wav'), 
(3, 'Nature', 15, 'birds.wav'),
(4, 'Bells', 8, 'church_bell.wav'),
(5, 'Bells', 12, 'doorbell.wav');
-- NOW GROUP BY is useful!
SELECT 
    category,
    COUNT(*) as number_of_sounds,
    AVG(duration_seconds) as avg_duration,
    SUM(duration_seconds) as total_duration
FROM sound_effects
GROUP BY category;
-- Result:
-- Nature: count=3, avg=30, total=90
-- Bells:  count=2, avg=10, total=20
```

# Connecting Tables
## Direct FOREIGN KEY

```sql
-- Your categories table (already perfect!)
CREATE TABLE IF NOT EXISTS categories (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  -- ...
);

-- The sounds table with foreign key to categories
CREATE TABLE IF NOT EXISTS sounds (
  id TEXT PRIMARY KEY, -- using bbc sound string id as id here
  category_id INTEGER NOT NULL,
  favorite BOOLEAN DEFAULT 0,
  -- [...]
  
  -- This creates the relationship!
  FOREIGN KEY (category_id) REFERENCES categories(id) ON DELETE CASCADE,
  
  -- Ensure we don't have duplicate sounds
  -- This ensures a combination of id and category_id remains unique
  UNIQUE(id, category_id)
);

-- Create indexes for fast lookups
CREATE INDEX idx_sounds_category 
	ON sounds(category_id);
CREATE INDEX idx_sounds_favorite 
	ON sounds(favorite) WHERE favorite = 1;
CREATE INDEX idx_sounds_last_accessed 
	ON sounds(last_accessed);
```

## Junction Table

```sql
-- Categories table (same)
CREATE TABLE IF NOT EXISTS categories (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL UNIQUE
);

-- Sounds table (no category_id here)
CREATE TABLE IF NOT EXISTS sounds (
  id TEXT PRIMARY KEY,
  description TEXT NOT NULL,
  duration REAL,
  -- other sound metadata...
);

-- Junction table for many-to-many relationship
CREATE TABLE IF NOT EXISTS sound_categories (
  sound_id TEXT NOT NULL,
  category_id INTEGER NOT NULL,
  added_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  
  PRIMARY KEY (sound_id, category_id),
  FOREIGN KEY (sound_id) REFERENCES sounds(id) ON DELETE CASCADE,
  FOREIGN KEY (category_id) REFERENCES categories(id) ON DELETE CASCADE
);
```
- then example usage:
```python
# Get all sounds in "Aircraft" category
cursor.execute("""
    SELECT s.* FROM sounds s
    JOIN sound_categories sc ON s.id = sc.sound_id
    JOIN categories c ON sc.category_id = c.id
    WHERE c.name = ?
""", ("Aircraft",))

# Get categories for a specific sound
cursor.execute("""
    SELECT c.name, sc.confidence FROM categories c
    JOIN sound_categories sc ON c.id = sc.category_id
    WHERE sc.sound_id = ?
""", (sound_id,))
```
- indexes are better than linked lists because:
	- it is `(O(log n) vs O(n))`
	- relationships are clear and maintainable
	- no duplication of data

### Speeding up with INDEX

> Without indexes whenever you do a table query you are doing a full table scan.
> 
> They work like a library catalog.
> 
> You don't use them directly in your queries, they work in background, ***SQLite optimizer*** decides automatically whether to use an index or not.
> 
> *Important*: Indexes update themselves automatically.
> 
> *Important*: Indexes are automatically created for PRIMARY KEYs, you need to manually declare them otherwise. Notice this means you need to create them for connection columns in junction tables, see example below.

```sql
-- Example as above
CREATE TABLE IF NOT EXISTS categories (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL UNIQUE
);

CREATE TABLE IF NOT EXISTS sounds (
  id TEXT PRIMARY KEY,
  description TEXT NOT NULL,
  duration REAL,
  -- other sound metadata...
);

-- Junction table for many-to-many relationship
CREATE TABLE IF NOT EXISTS sound_categories (
  sound_id TEXT NOT NULL,
  category_id INTEGER NOT NULL,
  added_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  
  PRIMARY KEY (sound_id, category_id),
  FOREIGN KEY (sound_id) REFERENCES sounds(id) 
	  ON DELETE CASCADE,
  FOREIGN KEY (category_id) REFERENCES categories(id) 
	  ON DELETE CASCADE
);

-- Indexes for performance
CREATE INDEX IF NOT EXISTS idx_sc_category 
	ON sound_categories(category_id);
-- notice important below for why whe don't need index for sound_id
```
- ***IMPORTANT***!
	- `INDEX on sound_categories(sound_id)` IS REDUNDANT
	- this is because `INDEX for sound_id` is created automatically
	- in order to give fast lookup for it
	- `the REVERSE index` IS NOT created automatically
	- this one you need to establish yourself

> Observe running the same query on the same table *after and before* establishing an index.
- [ ] TODO: make some experiments with `EXPLAIN QUERY PLAN`
```sql
-- Without indexes (simulated by temporarily disabling)
EXPLAIN QUERY PLAN
SELECT s.* FROM sounds s
JOIN sound_categories sc ON s.id = sc.sound_id
JOIN categories c ON sc.category_id = c.id
WHERE c.name = 'Aircraft';
-- Output: SCAN categories, SCAN sound_categories, SEARCH sounds

-- After adding indexes
-- SAME QUERY AS ABOVE
-- Output: SEARCH categories USING INDEX, SEARCH sound_categories USING INDEX, SEARCH sounds
```
# Python DB-API Standard


```c
PEP 249 (Python Database API Specification v2.0) says:
- All database modules should have a `Cursor` class
- Cursors should have `fetchone()`, `fetchall()`, `fetchmany()` methods
- `fetchone()` 

So while `cursor.fetchone()` is implemented in `sqlite3`, every Python database driver implements the same method:
```

- `cursor.fetchone()`
	- fetches one row
	- returns a tuple or `None`
	- even if you request one column it will return a tuple
```sql
cursor.execute("SELECT name FROM categories WHERE name='Nature'")
row = cursor.fetchone()[0]
print(row)        # ('Nature',)  ← 1-element tuple
print(row[0])     # 'Nature'
print()
```

- `cursor.fetchmany()`
- `cursor.fetchall()`

```python
import sqlite3
import psycopg2  # PostgreSQL
import pymysql   # MySQL
import cx_Oracle # Oracle

# All of these have cursor.fetchone() that returns a tuple!
sqlite_cursor.fetchone()     # From sqlite3 module
pg_cursor.fetchone()         # From psycopg2 module  
mysql_cursor.fetchone()      # From pymysql module
oracle_cursor.fetchone()     # From cx_Oracle module
```
# SQLite in python

## SQL-Specific

### Type Affinities

> Unlike most databases, SQLite uses ***type affinities***.

#### TIMESTAMP

> TIMESTAMP is a Type Affinity.
> 
> `TIMESTAMP` isn't a real SQLite type - it's just TEXT/INTEGER/REAL with a fancy name! SQLite stores it as TEXT (ISO8601 string) by default.

- You can [[#CREATE TRIGGER]]s around `TIMESTAMP`, for example:
	- `update_categories_timestamp` is the name of the trigger
	- your trigger target table:
		- is named in this case `categories`
		- you need to have `id` column
		- you need to have `last_updated` TIMESTAMP column
	- `CURRENT_TIMESTAMP` is a builtin sqlite function, others like that:
		- `CURRENT_DATE` 
		- `CURRENT_TIME`
		- `datetime('now')` same as `CURRENT_TIMESTAMP`
		- `julianday('now')` days since 4714 BC (used for date math)
	- `NEW` 
		- a special pseudo-table that contains the new version of the row being modified. 
		- In triggers, you get two magic tables:
		- `NEW` - The *row AFTER* the change (for INSERT/UPDATE)
		- `OLD` - The *row BEFORE* the change (for UPDATE/DELETE)
```sql
"Your table"
CREATE TABLE IF NOT EXISTS categories (
	id INTEGER PRIMARY KEY AUTOINCREMENT,
	name TEXT NOT NULL UNIQUE CHECK(LENGTH(name) > 0),
	size INTEGER CHECK(size > 0)
	last_updated TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)

"Your trigger"
CREATE TRIGGER IF NOT EXISTS update_categories_timestamp 
AFTER UPDATE ON categories
BEGIN
	UPDATE categories 
	SET last_updated = CURRENT_TIMESTAMP 
	WHERE id = NEW.id;
END;

"""
AFTER UPDATE ON categories → Trigger fires when ANY row is UPDATED
NEW.id refers to the ID of the row that was just updated
WHERE id = NEW.id ensures we ONLY update the timestamp on THAT specific row
"""
```
## Processes

### Check if data exists and isnt too old

```python
# Check if table has data and isn't too old
 
	
# Return cached data
cursor.execute("SELECT name, size FROM categories ORDER BY name")
return {name: size for name, size in cursor.fetchall()}
```