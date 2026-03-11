
# Eigenschaften

***Interpretierte Sprache***

- Der Code wird 
	- *Zeile für Zeile* 
	- *direkt aus dem Quellcode* 
	- *vom Python-Interpreter* gelesen und direkt ausgeführt.
- Python braucht *keine vorherige Kompilierung in* ***Maschinensprache***.
	- Im Gegensatz zu Sprachen wie C++, C# oder Java.
- Der Code kann sofort ausgeführt werden, was die Entwicklung beschleunigt.
- Andere Beißpiele für Interpretierte Sprache sind JavaScript, PHP oder Ruby.

# Types

### `arrays`
```python
sounds = ["thunder", "rain", "wind", "birds", "waves"]

print(sounds[0])  # "thunder"
print(sounds[1])  # "rain"

# ["thunder", "rain"]  # Start at 0, end BEFORE 2
print(sounds[:2])

# ["rain", "wind"] # Start at 1, end BEFORE 3
print(sounds[1:3])

# ["wind", "birds", "waves"] # Start AT 2, go to end
print(sounds[2:])   

# ["wind", "birds", "waves"]  # Last 3 items
print(sounds[-3:])

# Every other result (step)
every_other = sounds[::2]
# Reverse the list
reversed_results = sounds[::-1]

# mixed arrays are ALLOWED in python
mixed = [1, "hello", 3.14, True]

# Adding items
sounds = []
sounds.append("thunder")    # ["thunder"]
sounds.append("rain")       # ["thunder", "rain"]
sounds.insert(0, "wind")    # ["wind", "thunder", "rain"]

# Removing items
# POP IS USED FOR BEGINNING AND END OF ARRAY
sounds.remove("thunder") # Remove by value
last = sounds.pop() # Remove and return last item
first = sounds.pop(0) # Remove and return first item

# List length
len(sounds)  # Number of items

# Or create a new list
sounds.clear()  

# Check if item exists
# NO BUILTIN .contains("item") METHOD
if "rain" in sounds:
    print("Found it!")
```

### `classes`
```python
class CookieCutter:
    def __init__(self, shape="circle"):
        self.shape = shape  
    
    def print_shape(self):
        print(f"I make {self.shape} cookies!")
    

defaultcookie = CookieCutter()
gingerbread = CookieCutter("gingerbread man")  
star = CookieCutter("star")

# "I make gingerbread man cookies!"
gingerbread.print_shape()
# "I make star cookies!"
star.print_shape()         
```
- the constructor: `__init__(self, ...parameters)`
	- the `self` is obligatory in its definition
	- *you define object constructor parameters here*
- `self`
	- ***notice every class function has to take it as a first parameter***
		- 'it is how python knows which object you are talking about'
	- `self.shape = "star`
		- writing ***instance variables*** into the object

### `dictionaries`

> Dictionaries are important for example for `json` in python, because it interprets json objects `{}` as *dictionaries*, and json arrays  `[]` as python *lists*.

> Contrary to C# ***key and value types can change per item***, with only rule that key must be hashable:
```python
# Python - Keys must be hashable, but can mix types
d = {
    1: "integer key",           # ✅ int is hashable
    "two": "string key",        # ✅ str is hashable
    (3, 4): "tuple key",        # ✅ tuple is hashable
    # [5, 6]: "list key"        # ❌ list is UNhashable (mutable)
}

# Values can be ANYTHING - no restrictions!
d2 = {
    "a": 42,                    # int
    "b": "hello",               # str
    "c": [1, 2, 3],             # list
    "d": {"nested": "dict"},    # dict
    "e": print,                 # function
    "f": None                    # None
}
```

> creating, setting, modifying values
```python
# Empty dictionary
empty_dict = {}

# From dictionary comprehension
squares = {x: x**2 for x in range(5)}  
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}

# With initial values
person = {
    "name": "Alice",
    "age": 30,
    "city": "London"
}

# From list of tuples
person = dict([("name", "Alice"), ("age", 30), ("city", "London")])

# Add new or modify existing
person["email"] = "alice@example.com"

# Update multiple keys at once
person.update({"city": "Paris", "phone": "123-4567"})

# Remove items
del person["phone"] # Remove specific key
age = person.pop("age") # Remove and return value
last_item = person.popitem() # Remove and return last (key, value)

# Clear everything
person.clear()  # Empty dict
```

> getting values
```python
print(person["name"])

# Method 2: .get() (returns None or default if missing)
print(person.get("name"))

# Method 3: Check if key exists
if "age" in person:
    print("Age exists!")
```

> dictionary methods
```python
keys = person.keys()
for key in person.keys():
    print(key)
    
values = person.values() 

# Get all key-value pairs
"THIS IS IMPORTANT, U WILL OFTEN DO DICT. INTERPOLATION ON IT"
items = person.items()
# dict_items([('name', 'Alice'), ('age', 30), ('city', 'London')])

for key, value in person.items():
    print(f"{key}: {value}")
    
# Merge (Python 3.9+)
merged = dict1 | dict2
    
# Length
print(len(person)) 
```

> keys(), values(), items()
> 
> They are ***view objects*** - dynamic windows into the dictionary that update when the dictionary changes.
```python
my_dict = {"a": 1, "b": 2, "c": 3}

keys = my_dict.keys()     # dict_keys(['a', 'b', 'c'])
values = my_dict.values() # dict_values([1, 2, 3])
items = my_dict.items()   # dict_items([('a', 1), ('b', 2), ('c', 3)])

print(type(keys))   # <class 'dict_keys'>
print(type(values)) # <class 'dict_values'>
print(type(items))  # <class 'dict_items'>

# They are NOT lists - but they are iterable
# ✅ You can iterate over them
for key in my_dict.keys():
    print(key)

# ✅ You can check membership
if "a" in my_dict.keys():
    print("Found!")

# ✅ You can get their length
print(len(my_dict.keys()))  # 3

# ❌ You CANNOT index them
my_dict.keys()[0]  # TypeError!

# If you need actual lists:
key_list = list(my_dict.keys())
value_list = list(my_dict.values())
item_list = list(my_dict.items())

# Now you can index
print(key_list[0])    # 'a'
print(value_list[1])  # 2
print(item_list[2])   # ('c', 3)
```

`nested dictionaries`

### None

```python
# None vs False confusion
value = None
if not value:
	# True - None is falsy
if value is False:
	# 🔥 False - None is not False
	# you could have expected it to be true here
```
### strings

#### if string

- `""`  empty string evaluates to *FALSE*
- `None` evaluates to *FALSE*
- `"meow"` non-empty string evaluates to *TRUE*
- chence the most typical python way to check if a string is null or empty is:
```python
if not my_string:
	# stuff to happen when it does not exist
```

#### join()

`strings: join()`
```python
# Different separators
words = ['Hello', 'World']
print(' '.join(words))     # Hello World
print('...'.join(words))   # Hello...World
print(''.join(words))      # HelloWorld
print(' potato '.join(words)) # Hello potato World
print('\n'.join(words)) # separate with new lines

# Works with any iterable
print(', '.join('ABC'))    # A, B, C (strings are iterable!)
print(' | '.join(['x', 'y', 'z']))  # x | y | z

# Numbers need conversion first
numbers = [1, 2, 3]
print(', '.join(map(str, numbers)))  # 1, 2, 3
```

# Structures

### `for-loops`
```python
# using range()
for i in range(10):
	print(i) # 0,1,2...,9
	
for i in range(10, -1, -1):
	print(i) # 10,9,...,0

# using enumerate()
x = [5, "cat", 4.1]
for i, element in enumerarte(x):
	print(i, element) # 0 5, 1 "cat", 2 4.1 
```

### `function defs`
```python
def meow():
	print("meow")
	
def add(x,y):
	return a + y
	
def getstuff():
	return "meow", 23
	
x, y = getstuff()
```

### if

#### truthy vs. falsy
| Value   | `if not value` | `value is None` | `value == False` |
| ------- | -------------- | --------------- | ---------------- |
| `None`  | ✅ True         | ✅ True          | ❌ False          |
| `False` | ✅ True         | ❌ False         | ✅ True           |
| `True`  | ❌ False        | ❌ False         | ❌ False          |
| `0`     | ✅ True         | ❌ False         | ❌ False          |
| `""`    | ✅ True         | ❌ False         | ❌ False          |
| `[]`    | ✅ True         | ❌ False         | ❌ False          |

### `list comprehension`

```python
LIST COMPREHENSION RECIPE:
┌─────────────────────────────────────────┐
│ [ WHAT    for   EACH   in   SOURCE ]    │
│  ↑              ↑          ↑            │
│  │              │          └─ Your list │
│  │              └─ Temporary variable 🌱│
│  └─ What you want to collect            │
└─────────────────────────────────────────┘
```

`list comprehension: basics`
```python
# filtering an array for numbers divisible by 2
# using modulo operator
# but obvs this could be anything else
a = [1, 2, 3, 4, 5]
res = [val for val in a if val % 2 == 0]
# res: [2, 4]

# apply expressions on each val and save that to new array
a = [1, 2, 3, 4, 5]
res = [val * 2 for val in a]
# res: [4, 9, 16, 25]
```

`list comprehension: json example`
- Assume you have following json:
```json
data = {
    "total": 601,
    "results": [  # list!      
        { # dict at index 0!              
            "id": "07069030",
            "description": "Exterior. Taxi up & past.",
            ...
        },
        { # dict at index 1!
            "id": "07069029", 
            "description": "Passing overhead.",
            ...
        },
        ... # more dicts
    ]
}
```
- You want to get an array with just descriptions from the data
```python
descriptions = 
	[el['description'] for el in data['results']]
```
- This means you need to remember you can perform modifying ***operations on both input and output*** of the list comprehension!
```python
# KEY INSIGHT #1: 
# List comprehension = loop + append in one line

# Traditional way:
#   descriptions = []
#   for sound in data['results']:
#       descriptions.append(sound['description'])

# KEY INSIGHT #2: You can modify BOTH the input and output
# Modify the output (what you collect):
short_descs = [sound['description'][:20] for sound in data['results']]
upper_descs = [sound['description'].upper() for sound in data['results']]

# Modify the input (what you loop over):
first_three = [sound['description'] for sound in data['results'][:3]]
long_sounds = [sound['description'] for sound in data['results'] if sound['duration'] > 60000]
```

### unpacking (js destructuring)

> basics
```python
# Tuples (most common with databases)
coordinates = (10, 20)
x, y = coordinates  # x=10, y=20

# Lists (arrays)
scores = [95, 87, 92]
first, second, third = scores  # first=95, second=87, third=92

# Strings (unpacks characters)
word = "cat"
a, b, c = word  # a='c', b='a', c='t'

# Sets (order not guaranteed!)
colors = {"red", "green", "blue"}
c1, c2, c3 = colors  # Works, but order unpredictable

# Dictionaries (unpacks keys)
person = {"name": "Alice", "age": 30}
key1, key2 = person  # key1='name', key2='age'

# Ranges
first, second, third = range(3)  # first=0, second=1, third=2

# Generators
gen = (x**2 for x in range(3))
a, b, c = gen  # a=0, b=1, c=4
```

> selective techniques
```python
# Basic unpacking (length must match)
data = [1, 2, 3]
a, b, c = data  # Works: a=1, b=2, c=3

# Star unpacking (when lengths might vary)
data = [1, 2, 3, 4, 5]
first, *middle, last = data  
# first=1, middle=[2,3,4], last=5

# Ignoring values with underscore
data = [10, 20, 30, 40]
first, _, third, _ = data  # first=10, third=30

# Nested unpacking
matrix = [[1, 2], [3, 4]]
(a, b), (c, d) = matrix  # a=1, b=2, c=3, d=4
```

> real-world examples
```python
# 1. Database query (returns tuple)
# Notice we don't need .fetchall() here
# Cursor itself is iterable
# It automatically calls .fetchone() on every row
# This is actually better than fetchall() because it doesnt load everything at once into memory
cursor.execute("SELECT id, name, duration FROM sounds")
for id, name, duration in cursor:  # Each row is a tuple
    print(f"{id}: {name} ({duration}s)")

# 2. Parsing CSV (returns list)
for row in csv.reader(file):
    name, age, city = row  # row is a list

# 3. Function returning multiple values
def get_stats():
    return 42, "active", 3.14  # Returns a tuple

count, status, ratio = get_stats()

# 4. Swapping variables (works with any iterable)
a, b = b, a  # Python creates a tuple (b, a) then unpacks
```

# Intermediate

### doscstring

- they can be made for `entire modules, classes or function definitions`

```python
def add_sample(title, url, duration=None):
    """Add a BBC audio sample to the database.
    
    Args:
        title (str): The sample title
        url (str): Direct URL to audio file
        duration (int, optional): Duration in seconds
    
    Returns:
        int: The database ID of the new sample
    
    Raises:
        ValueError: If URL is invalid
    """
    # Function code here...

# Later, you can access the docstring:
print(add_sample.__doc__)
# Or in interactive Python:
# help(add_sample)
```

### flags

- `python3 -m src.main mark_favourite`
	- it is used to target [[#module]]s
	- without it will look in current directory and you need to target a specific python script like `src/main.py`
		- relative to the current directory
	- it will look instead in `PYTHONPATH` and in `System-Pfade`
### import

```python
def export_to_excel(data):
    """Export data to Excel if pandas is available"""
    try:
	    # Try importing only when needed
        import pandas as pd  
        df = pd.DataFrame(data)
        df.to_excel('output.xlsx')
        return True
    except ImportError:
        print("⚠️  Install pandas: pip install pandas")
        return False

# Program works even if pandas isn't installed
# Only fails when export_to_excel() is called
```

### module

#### `PYTHONPATH`

- Wie Python Module findet:
```python
import sys
# Zeigt alle Verzeichnisse, in denen Python sucht
print(sys.path)  

# 
```
- Typische `sys.path` (vereinfacht):
	1. Das Verzeichnis des aktuellen Skripts
	2. `PYTHONPATH` (falls gesetzt)
	3. Standard-Bibliotheken
	4. Site-Packages (installierte Pakete)
- `PYTHONPATH` in bash setzen:
```bash
#!/bin/bash

PROJECT_ROOT="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

# Wenn PYTHONPATH nicht existiert, nimm leeren String
export PYTHONPATH="${PYTHONPATH:-}:$PROJECT_ROOT"

echo "🔧 PYTHONPATH: $PYTHONPATH"
python3 -m src.main function_name
```
### venv

> If vscode does not recognize some of your imports (missing packages) even though they correctly list in `pip list` inside that venv, then:
> 
> Run in vscode `Python: Select Interpreter` and select the interpreter `python.exe` located inside of your venv.
> 
> OR BETTER YET: Make sure you dont just view the file but instead press `Open (as?) folder` one you are in vscode.
> 
> ALSO: Make a habit of `Reload Window`

***Process***:
- go to the location where you want to create a venv
- `python3 -m venv .venv`
	- this will create a venv called `.venv`
	- it is a commonly used name, like default gitignores for python will assume that
	- there are more advanced solutions for different venvs like different ones for differenct production environments, but you dont need to worry about that now
	- what gets created:
```python
venv/
├── bin/ # Executables (activate script, python, pip)
├── lib/ # Installed packages go here
└── pyvenv.cfg # Configuration file
```
- `source venv/bin/activate`
	- this activates the venv, you must do it whenever you wish to start working in it
	- this is how you know it is activated:
```bash
# Before activation:
$ which python
/usr/bin/python

# After activation:
(venv) $ which python
/home/you/projects/bbc-sound-browser/venv/bin/python
# See the (venv) prefix!
```
- `deactivate`
	- will deactivate the venv

***Common things to do inide the venv***:

- `source venv/bin/activate`
	- ***dont forget!***
- `python --version`
	- check the used python version inside the venv
- `pip install/uninstall beautifulsoup4 requests`
	- install two packages into that venv
- `pip install --upgrade beautifulsoup4`
	- upgrade a package
- `pip list`
	- will show you installed packages
- `python sound_browser.py`
	- will run python script inside that venv

***Sharing your project***:

`pip freeze > requirements.txt`
`pip install -r requirements.txt`
```bash
# Save your dependencies
(venv) $ pip freeze > requirements.txt

# Send requirements.txt to a friend
# They can recreate your environment:
git clone your-project
cd your-project
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

***Automate activation***:

```bash
# Auto-activate when entering project directory
function cd() {
  builtin cd "$@"
  if [[ -f "venv/bin/activate" ]]; then
    source venv/bin/activate
  fi
}
```
### `__name__`

> `if __name__ == "__main__":` *is one of the most important Python concepts*! 
> 
> It controls whether your code runs when executed directly vs. when imported as a module.

- When you *RUN* the file directly: 
	- `__name__` equals *__main__*
- When you *IMPORT* the file: 
	- `__name__` equals ***the module name***

```python
if __name__ == "__main__":
    if len(sys.argv) < 2:
        print(__doc__)
    else sys.argv[1] == "add":
        add_sample_cli()
```

### `__init__.py`

> *Think of `__init__.py` as the package's "front door"* - it defines how the package looks to the outside world.

- It can be empty
- TODO: example
```python
# src/__init__.py

# 1. Control what gets imported with "from src import *"
__all__ = ['AudioDatabase', 'BBCApiClient']

# 2. Import key classes for easier access
from .database import AudioDatabase
from .api_client import BBCApiClient

# 3. Package-level variables
VERSION = "1.0.0"

# Now users can do:
from src import AudioDatabase, VERSION
```
# Standardbibliothek

### built-in

> List of built-in Python functions.
> 
> https://docs.python.org/3/library/functions.html
#### open

> Note that some of these things can be more simply done with [[#Path]]. 
> 
> Especially reading with just `data = path.read_text()`

```python
# default is read & text or 'r'
with open(filepath) as f: 
	data = f.read()
# equivalent to:
with open(filepath, "rt") as f:

# with 'w' works in hard overwrite mode, watch out
# example with json
with open(filepath, "w", encoding="utf-8") as f:
	json.dump(data, f, indent=2, ensure_ascii=False)

# append with 'a'
# for text files, 't' is assumed here
with open(filepath, 'a') as f: 
	f.write(data)
	
# 'x' for exclusive creation
# SAFE, failing if file already exists
with open(filepath, 'xw') as f:
	...
	
# notice open can work with Path
filepath = Path("data/myjson.json")
with filepath.open("w") as f:
	f.write(data)
	
# for binary files you will need 'b'
filepath = Path("sounds/meow.mp3")
with filepath.open("wb") as f:
	f.write(data)
```

### argparse

> A clean way to deal with arguments passed to the python module/script.

```python
# This description would show when someone would would put --help after your script call.
parser = argparse.ArgumentParser(
	description="BBC Sound Effects Browser")

# Positional / Rqquired argument
# Usage: python main.py browse
parser.add_argument(
	"command", 
	choices=["get_categories", "search", "browse"])

# Optional argument - !! can be omitted !!
# it is defined by putting -- or - in front of it
# Usage: python main.py search --query "rain"
#        python main.py search -q "rain"
parser.add_argument(
	"--query", 
	"-q", 
	help="Search query")

# numerical
parser.add_argument("-n", "--number", type=int)

# Optional argument - flag
# Does not take argument
# Usage: python script.py -v
parser.add_argument("-v", "--verbose", action="store_true")

args = parser.parse_args()
# If user runs: python main.py search -v -q "rain"
# Then:
print(args.command)  # "search"
print(args.query)    # "rain"
print(args.verbose)  # True, just because -v is present

if args.command == "get_categories":
	# do one thing
    
elif args.command == "browse":
	# do something else
```

### json

#### read / save from file

- reading/writing json using `Path` or `open()`
	- see [[#Path]]
	- see [[#open]]
```python
p = Path("data.json")

# 📂 READING
with p.open() as f:
    data = json.load(f) # json.load() "load FILE OBJECT"
    
# alternative with Path using json.loads() "load STRING"
text = p.read_text()
data = json.loads(text)

# 📂 WIRITING with Path
# again notice .dump() vs. dumps() since the second returns a string
text = json.dumps(data) 
p.write_text(text)

# 📂 WIRITING with 
with p.open("w", encoding="utf-8") as f:
	json.dump(data, f, indent=2, ensure_ascii=False)
```

### os

`os.makedirs('data', exist_ok=True)`

### pathlib

#### Path

> This is how you get it.
> 
> `from pathlib import Path`

> home and `/` operator
```python
home_path = Path.home()

# /home/username (Linux), /Users/username (macOS), C:\Users\username (Windows)
print(home_path)  

# relative to where the script runs
# both below are the same
# but ./ is better because it is explicit
relative_path = Path('cache')
relative_path = Path('./cache')

# Path overrides / operator and lets you neatly connect paths like that
# It is still a Path object, not a string!
file_path = home_path / "docs" / "file.txt"
print(type(file_path))  # <class 'pathlib.PosixPath'>

```

> mkdir
```python
# parents: is necessary if you want to create nested directories, default is no
# exist_ok: if the dir already exists it will not throw err, you will most likely always use it lol cos default is False
Path("temp/trash/json").mkdir(parents=True, exist_ok=True)

# make sure to not make this mistake
json_path = Path('./docs') / 'json_responses' / 'data.json'
json_path.mkdir(parents=True, exist_ok=True)
# above will crate data.json directory
# u must instead:
dir_path = Path('./docs') / 'json_responses'
dir_path.mkdir(parents=True, exist_ok=True)
json_path = dir_path / 'data.json'
```

> parent & stems & similar
```python
from pathlib import Path
p = Path("/home/user/projects/data/file.txt")
print(p.parent)       # /home/user/projects/data
print(p.parent.parent) # /home/user/projects
print(p.parent.parent.parent) # /home/user

print(p.stem)      # "file" (no extension)
print(p.name)      # "file.txt" (full filename)
print(p.suffix)    # ".txt" (just the extension)
print(p.suffixes)  # ['.txt'] (for multiple extensions like .tar.gz)

p = Path("/home/user/docs/report-final-v3.tar.gz")
print(p.parts) # ('/', 'home', 'user', 'docs', 'report-final-v3.tar.gz')
```

> read / write text
```python
path = Path('./asd') / 'sometextfile.json'
text = path.read_text()

# can also write to it
path.write_text(text)
```
### sqlite

#### basics

```
connect() → get cursor() → execute() SQL → commit() → close()
```

- `conn = sqlite3.connect('mydb.db')`
	- Es öffnet eine Verbindung zu einer SQLite-Datenbankdatei.
	- Wen die Datei `mydb.db` nicht existiert, wird sie von SQLite erstellt.
	- Das *stellt* die Kommunikation zwischen Python und der SQLite-Datenbankdatei *her*.
- `cursor = conn.cursor()`
	- *Why "cursor"?*: The name comes from database terminology. A cursor is like a "pointer" or "marker" that moves through your database results.
	- It creates a workspace where you can execute commands.
	- You must have a cursor to execute SQL commands.
- `cursor.execute('SELECT * FROM books')`
	- You are reading or writing wherever your cursor, 'finger', is pointing.
- `books = cursor.fetchall()`
	- You are copying everyting you are pointing at.
- `conn.commit()`
	- Save the changes.
- `conn.close()`
	- Leave the library.

#### with

```python
with sqlite3.connect('audio_favorites.db') as conn:
    cursor = conn.cursor()
    cursor.execute('''...SQL here...''')
    # No explicit commit() needed!
    # Auto-commits on successful exit
    # Auto-rollbacks if exception occurs
```

#### cursor.execute(...)

> Things like `CREATE TABLE, INSERT, UPDATE, DELETE` require `commit()` or the changes will be lost!

```python
# single line execute
cursor.execute('SELECT * FROM favorites')`

# multiline execute
cursor.execute('''
CREATE TABLE IF NOT EXISTS favorites (
    id INTEGER PRIMARY KEY,
    title TEXT NOT NULL,
	added_date DATE DEFAULT CURRENT_DATE
)
''')
```

> parametrized queries
```python
user_name = "Alice"
score = 100

# using ? + tuple
cursor.execute(
	'INSERT INTO users (name, score) VALUES (?, ?)',
	user_name, score))

# named + dictionary
cursor.execute(
    'INSERT INTO users (name, score) VALUES (:name, :score)',
    {'name': user_name, 'score': score}
)
```

> executemany
```python
# Multiple inserts
users_data = [
    ("Bob", 85),
    ("Charlie", 92),
    ("Diana", 78)
]
cursor.executemany(
	'INSERT INTO users (name, score) VALUES (?, ?)',
	users_data)
```

#### fetch and iteration

```python

# ❌ BAD: Loading everything into memory
# Loads ALL rows into memory at once!
cursor.execute("SELECT * FROM sounds")
all_rows = cursor.fetchall()  
for row in all_rows:
    process(row)
    
# ✅ GOOD: Streaming rows one at a time
# Fetches rows on demand, memory efficient
cursor.execute("SELECT * FROM sounds")
for row in cursor:  
    process(row)
    
# ✅ ALSO GOOD: Explicit fetching
# Python 3.8+ walrus operator
while row := cursor.fetchone():  
    process(row)

# Database query (returns tuple)
# Notice we don't need .fetchall() here
# Cursor itself is iterable
# It automatically calls .fetchone() on every row
# This is actually better than fetchall() because it doesnt load everything at once into memory
cursor.execute("SELECT id, name, duration FROM sounds")
for id, name, duration in cursor:  # Each row is a tuple
    print(f"{id}: {name} ({duration}s)")
```

#### error handling

`sqlite3.OperationalError`
```python
def create_table_safely():
    try:
        conn = sqlite3.connect('test.db')
        cursor = conn.cursor()
        
        # This will fail - missing comma!
        cursor.execute('''
            CREATE TABLE IF NOT EXISTS test (
                id INTEGER PRIMARY KEY
                name TEXT NOT NULL  -- MISSING COMMA!
            )
        ''')
        
        conn.commit()
        print("Table created")
        
    except sqlite3.OperationalError as e:
        print(f"❌ SQL Error: {e}")
        print("Check your commas and syntax!")
    finally:
        conn.close()
```

### zipfile

```python
import zipfile
from pathlib import Path
def unzip_file(zip_path, extract_path=None):
    """
    Extract a zip file
    
    Args:
        zip_path: Path to the zip file
        extract_path: Where to extract (default: same directory as zip)
    """
    zip_path = Path(zip_path)
    
    if extract_path is None:
        extract_path = zip_path.parent / zip_path.stem
    
    extract_path = Path(extract_path)
    extract_path.mkdir(parents=True, exist_ok=True)
    
    with zipfile.ZipFile(zip_path, 'r') as zip_ref:
        zip_ref.extractall(extract_path)
    
    print(f"✅ Extracted {zip_path.name} to {extract_path}")
    return extract_path
# Usage
unzip_file("archive.zip")
unzip_file("sounds.zip", "./my_sounds/")
```
# Common Libs

### requests

```python
version = "0.1.0"

base_url = "https://sound-effects-api.bbcrewind.co.uk/api/sfx/search"

# example headers
headers = {
f'User-Agent': 'bbc-sound-browser/{version} (educational project) ',
'Content-Type': 'application/json',
'Accept': 'application/json'
}

# example payload, you can search for what the api expects by messing around with the webpage and watching in inspector tools Network Monitor -> XHR for GET or POST requests, POST is the one that will need a payload and in Request Body you can peek into its expected structure and reproduce it for your needs
payload = {
	"criteria": {
	"from":0,
	"size":40,
	"tags": None,
	"categories":["Animals"],
}

response = requests.post(base_url, json=payload, headers=headers)

# gives you response content in bytes
bytes_data = response.content
# gives you text data
text_data = response.text
# parses response content into json format
json = response.json()

}
response = request
```

`json: example for a caching function`
- also checking what format of data user passed into it
```python
def cache_json(self, data, filename):
    cache = "cache/json"
    path = Path(cache)
    path.mkdir(parents=True, exist_ok=True)
    filepath = path / filename
    
    # If you passed response.content (bytes)
    if isinstance(data, bytes):
        # Decode bytes to string, then parse to dict
        data = json.loads(data.decode('utf-8'))
    # If you passed response.text (string)
    elif isinstance(data, str):
        data = json.loads(data)
    # If you passed response.json() (dict/list) - already good!
    
    with open(filepath, 'w', encoding='utf-8') as f:
        json.dump(data, f, indent=2, ensure_ascii=False)
```