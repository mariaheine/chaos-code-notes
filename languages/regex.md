# Tiny Regex cheatsheet

## 🌼 SINGLE CHAR



- `\d` 
	- 0-9 number digit \D anything but 0-9
- `\w` 
	- A-Z a-z 0-9 - *any character number or word* \W anything but \w
- `\s` 
	- white space \S ...
	- TODO: difference with capital S?
- `.` 
	- any character *other than newline*
	- 🔥 but you u use `SingleLine regex option` it will *also match new lines*

  

🔥 \n new line matching is problematic, use \s if it could be enough

https://stackoverflow.com/questions/6109882/regex-match-all-characters-between-two-strings


  

Read more on matching linebreaks: [https://stackoverflow.com/questions/20056306/match-linebreaks-n-or-r-n](asdfa)

  

## 👾 FORBIDDEN CHARACTERS

  

` - this little thing gave me much headache, it does something weird with c# regex when you try to match for it directly

  

## 🌺 QUANTIFIERS

  

```

* 0 or more

+ 1 or more (at least one)

? 0 or 1 (*optional character*)

{min,max} min to max amount

{n} n amount

```

  

### 🚧 Be wary of greedy vs lazy quantifiers

  

Greedy will select as much as possible.

Lazy will stop at first chance.

  

Adding `?` to a quantifier will make it lazy.

  

Quantifiers examples:

```html

colou?rs? -> this will find all existing colour, color, colours, colors words in a text

-> note that quantifiers follow a single char character

\w{5} -> will match all sequences of 5 characters

  

Notice the difference between what is selecteds in word stackoverflow below

<greedy> s.*o

<lazy> s.*?o

  

stackoverflow

```

  

## ⭐ POSITION

```

^ beginning

$ end

\b word boundry

```

  

Postions example:

```

^\w+ -> Will find all words at the beginning of a line

\w+$ -> will match any sequence of \w characters ending the line

\b\w{5}\b -> note how this will improve on above `\w{5}`

```

  

## 💠 CHARACTER CLASS

  

*When there is a specific group of characters you are interested in matching*

  

```

[abc] -> match a or b or c

[-.] -> match - or . (*neither dot or dash are metacharacters in this case, they are treater literally*)

[a-c] -> match any letter from a to c (*note dash works again as a meta character here*)

[a-c0-5] -> match any a to c or 0 to 5

[^a-z] -> *ANYTHING* but characters a-z, ^ has a completely different meaning inside a class

```

  

## 🐛 ALTERATION

  

*either or*

  

```

(com|edu|eu) -> match either com or edu or eu

```

****

## 🚇 CAPTURING GROUPS

  

This is achieved with [match groups](https://regexone.com/`lesson/capturing_groups).

  

```c

(random regex here) -> "in this case parenthesis are used to create groups of sequences"

"which can be later referred to using $n sign"

$0 -> "always refer to everything (*whole selection*)"

$1 -> "first subgroup created by parenthesis"

\1 -> "first subgroup backreferenced during search

this might be actually useful when searching for a sequence of same characters"

  

"A group can also be nameed! (so then you can retrieve it by name and not an index):"

(?<groupName>your_regex_here)

```

  

And in C#: (remember use index 1 to match the first group, 0 takes entire regex match)

  

```c

string input = "OneTwoThree";

  

Regex r1 = new Regex(@"One([A-Za-z0-9\-]+)Three");

  

Match match = r1.Match(input);

if (match.Success)

{

string v = match.Groups[1].Value; //

Console.WriteLine("Between One and Three: {0}", v);

}

```

  
  

## ⛺ LOOKAHEAD/BEHIND

  

*when there are some conditions as to the surroundings of what you want to match*

  

```c

lookahead:

X(?=Y) -> positive: "look for X, but match only if followed by Y"

-> negative: "search X, but only if <<not>> followed by Y".

  

lookbehind:

(?<=Y)X -> positive: "matches X, but only if there’s Y before it"

(?<!Y)X -> negative: "matches X, but only if there’s no Y before it"

```

  

## 🚁 ESCAPE CHARACTER

  

*when u need to match one of the special characters like a dot '.'*

```

\ -> use it whenever you need to use one of the meta characters

```

  

## 🧁 IN C#

  

class `Regex`

  

```

Regex expression = new Regex(@"your expression goes here");

```

  

class

  

## 🍪 TEST TEXT

  

Match all phone numbers in text below: `\(?\d{3}[-.)]\d{3}[-.]\d{4}`

  

Match any 4 letter words (case insensitive): `\b[A-Za-z]{4}\b`

  

Match any email adress: `\w+[\w.]\w+@\w+\.(com|edu|eu)`

  

Mass replace list of names to show last name first `(\w+),\s+(\w+)` then replace with `$2, $1` (*watch out in current text it will *)

  

marai.heine@gmail.com

aiaiohhh@net.edu

stacy.fluff@fluff.eu

  

956.563.8503

654-971-3690

(512)564-9096

  

Stacy, Fluff

Valentina, Lovemoon

Iffy, Merryhill

  

Contrary to popular belief, Lorem Ipsum is not simply random text. It has roots in a piece of

classical Latin literature from 45 BC, making it over 2000 years old. Richard McClintock,

a Latin professor at Hampden-Sydney College in Virginia, looked up one of the more obscure

Latin words, consectetur, from a Lorem Ipsum passage, and going through the cites of the word

in classical literature, discovered the undoubtable source. Lorem Ipsum comes from sections

1.10.32 and 1.10.33 of "de Finibus Bonorum et Malorum" (The Extremes of Good and Evil) by Cicero,

written in 45 BC. This book is a treatise on the theory of ethics, very popular during the

Renaissance. The first line of Lorem Ipsum, "Lorem ipsum dolor sit amet..", comes from a line

in section 1.10.32.