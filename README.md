# Fearless-Regex (in Javascript)

## Introduction

```
var EMAIL_REGEX = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
```

If you see the code above and if you are like me, you might get headache by the mere sight of it. Instead of fleeing Regex like one would run away from a cave troll, we need to be fearless and conquer it.

![sam vs cave troll](/assets/images/sam_cavetroll.gif)

This is a tutorial I pulled from various resources. I try to make it as simplistic and progressive as possible so everyone can follow along. The main purpose of this is for my own learning (I learn best when I can explain it to other people - you should do the same!). While doing so, I hope that you readers will benefit greatly as well. If you see any mistake, be it ~~grammer~~ grammar mistake or conceptual mistake, please don't hesitate to let me know.


## Creating Regex

Therer are 2 ways to create Regex:

1. Regex literal. Example: `var re = /ab+c/`

2. Regex constructor (RegExp object). Example: `var re = new RegExp(‘ab+c’)`

## Regex Patterns:

Regex has **two** types of characters: simple characters and special characters.

Example: `/abc/` is simple and `/Chapter(\d+)\.\d*/` is special.

1. Simple patterns: `/abc/` looks for characters `‘abc’` that occur together in that order. It matches `“Hey, show me your abc’s”` or `“Evolution of slabcraft”`. `“Grab crab”` will not match because the space between `"ab c"`s

2. Special characters. For example: `/ab*c/` matches any character combination where single a is followed by 0 or more b’s (`*` means 0 or more). Will match `“cbbabbbbcdefg”` and `“abbc”`


Here are some list of special characters in regex ([source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)). Don’t try to memorize them all. Know your limit. I can’t memorize them all in one day. I would suggest reading 3-5 per day and practice them until you nail it down. Then move on to the next set of 3-5.

Let’s start learning. There are about 30+ special characters. They are all pretty easy. Let’s look at the first 6. The information builds up over time, so make sure we are well-acquainted with all special characters we have seen so far. We will use `.match()` function on our examples.

#### \ :

a. Backslash that precedes non-special characters makes the next character special. It forms word boundary character (will explain later)

b. Backslash that precedes special character indicates next character is not special, should be interpreted literally. /a*/ means 0 or more a’s. /a\*/ means literally expecting a followed by an asterisk “a*”

c. Backslash backslash means literal backslash `/\\/` expects string of `“\”`


#### ^ :

It matches beginning input.

```
var str = “Hello regex”;
str.match(/^He/)         //=> [“He”]
str.match(/ello/)        //=> [“ello”]
str.match(/^ello/)       //=> null
```

#### $ :

It matches the end of input.

```
var str = “Hello regex”;
str.match(/Regex$/)     //=> [“Regex”]
str.match(/.*x/$)       //=> What will this print? Why?
```

#### * :

It matches preceding expression 0 or more times. Equivalent to {0,}.


```
var re = /no*/
var str = “nooooooooo!!!”
str.match(re)           //=> [“noooooooo”]

var re = /no*abc/
var str = “nooooo!!!”
str.match(re)           //=> ?

var str = “nooooooooabc”
str.match(re)           //=> ?

re = /no*/
str = “nooooo”
str.match(re);          //=> ?
```

#### + :

It matches preceding expression 1 or more times. Equivalent to {1,}


```
var re = /a+/;        //expects one or more a’s
var str = “”Mandy”;
str.match(re);        //=> [“a”]
str = “Whaaaaat!!?”
str.match(re);        //=> [“aaaaa”]
str = “mndy”
str.match(re);        //=> null
```


#### ? :

It matches preceding expression 0 or 1 time. Equivalent to {0,1}

```
var re = /e?le?/;
var str = “angel”;
str.match(re);        //=> [“el”]

str = “lemur”
str.match(re);        //=> ?

str = “lol”
str.match(re);        //=> ?
```


If ? is used after &#42;, +, ?, or {}, makes quantifier non-greedy. Just like the name implies, greedy means matching as many possible characters, while non-greedy means matching fewest possible characters.

For example, `/\d+/` will match one or more numbers. `\d` matches any decimal. `+` looks for 1 or more decimals.


```
var re = /\d+/
var str = “12345abcde”
str.match(re);          //=> [“12345”]

re = /\d+?/
str.match(re);          //=> [“1”]
```

`?` is also used in look-ahead assertions. Ex: x(?=y) and x(?!y). Explained later.



#### . :

It matches any single character except newline character

```
var re = /.n/;
var str = “man”
str.match(re); //=> [“an”]

str = “ion”;
str.match(re) //=> [“on”];

str = “no”;
str.mach(re); //=> ?

re = /.*n/
str = “ion”
str.match(re)

str = “no”
str.match(re)
```


Those are the basics. Let's talk about more advanced functions.

___

### Capturing

#### (x) :

The parentheses matches everything inside (for example, (x) matches x) and remembers it. The set of parentheses are called capturing parentheses.


```
'hello'.replace(/(e)(.*)(o)/, '$3$2a')          //=> 'holla'

'bar foo'.replace(/(...) (...)/, '$2 $1')       //=> 'foo bar'
'bar foo'.replace(/(...) (...)/, 'what')        //=> 'what'
'bar foo'.replace(/(...)/, 'what')              //=> 'what foo'
'bar foo'.replace(/(...) (what)/, '$2 $1')      //=> 'bar foo'
'bar foo what'.replace(/(...) (what)/, '$2 $1') //=> 'bar what foo'
```

 First, regex looks for the match, ex: `/(...) (...)/`. It is looking for sequence of 3 of (any) characters, followed by a space, then a sequence of any 3 characters again. In this case, if our string is `'bar foo'`, the first `(...)` captures `'bar'` (remembers it as `$1`) and second `(...)` captures `'foo'` (remembers it as `$2`).


 #### (?:x) :

 It matches x but does not remember it (non-capturing parentheses).


 ```
'foo bar'.replace(/(foo)/, '$1')
'foo bar'.replace(/(?:foo)/, '$1')

'foo bar'.match(/(...) (...)/)    //=> ["foo bar", "foo", "bar"]
'foo bar'.match(/(...) (?:...)/)  //=> ["foo bar", "foo"] (no bar)
 ```


For more information about non-capturing group, check out this [SO post](https://stackoverflow.com/questions/3512471/what-is-a-non-capturing-group-what-does-a-question-mark-followed-by-a-colon). It is a lot like capturing parentheses, but without the overhead of capturing them.


#### x(?=y) :

It matches only if x is followed by y. y values are not stored. It is called a *lookahead*.

```
'chris pratt'.match(/chris(?= pratt| hemsworth)/);  //=> ["chris"]
'chris evans'.match(/chris(?= pratt| hemsworth)/);  //=> null
```


#### x(?!y) :

It matches only if x is **not** followed by y. It is the opposite of lookahead and called negated lookahead.

```
/** first recall the lookahead **/
'ironman'.match(/iron(?=man)/)  //=> ["iron"]
'ironic'.match(/iron(?=man)/)   //=> null

/** as expected, if we switch to negated lookahead: **/
'ironman'.match(/iron(?!man)/)  //=> null
'ironic'.match(/iron(?!man)/)   //=> ['iron']
```

#### x | y :

It matches if string is either **x** or **y**.

```
"batman".match(/bat|ant/)     //=> ["bat"]
"megaman".match(/bat|ant/)    //=> null
```

#### {n, m} :

It matches *at least* **n** occurence and *at most* **m** occurence. There are two things to keep in mind:

1. Regex will match no more than the maximum amount of m, and will not match at all if it does not meet the minimum:

```
"fight on!".match(/i{1,3}/)     //=> ['i']
"fiiiiight on!".match(/i{1,3}/) //=> ['iii']
"punch on!".match(/i{1,3}/)     //=> null
```

2. m can be left blank `{n,}`. If it is blank, it will match n minimum and indefinite m.

```
"fiiiiiiight on!".match(/i{1,}/)  //=> ["iiiiiii"]
```

Note: `i{1,}` is the same as `+`.

3. m and the preceding comma can be left blank `{n}`. If it is so, it will match exactly n occurence.

```
"fiight on!".match(/i{2}/)    //=> ["ii"]
"fiiight on!".match(/i{2}/)   //=> ["ii"]
"fight on!".match(/i{2}/)     //=> null
```

#### [abc] :

It matches character set inside square brackets.

```
"n vwls hr".match(/[aeiou]/)        //=> null
"no vowels here?".match(/[aeiou]/)  //=> ["o"]
"no vowels here?".match(/[aeiou]/g) //=> ["o", "o", "e", "e", "e"]
```

Note, on the last example I put `g` after regex pattern. `g` means `global`. It tells regex to check every single character in the string instead of returning after the first match.

The bracket is also a good way to check for vowels, consonants, or number existence in a string.

```
"no number here".match(/[1-9]/)     //=> null
"1 number here".match(/[1-9]/)      //=> ["1"]
```

#### [^abc] :

It matches for **non**-character set. Opposite of the pattern above.

```
"abcde".match(/[^a-e]/)           //=> null
"abcdefg".match(/[^a-e]/)         //=> ["f"]
```
