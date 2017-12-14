# Fearless-Regex (in Javascript)

## Introduction

```
var EMAIL_REGEX = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
```

If you see the code above and if you are like me, you might get headache by the mere sight of it. Instead of running away like one would run away from a cave troll, I decided to put up a fight and learn it.

This is a tutorial I put up various resources. I try to make it as simplistic and progressive as possible (I try to not put a new symbol/ concept if it hasn't been discussed before), so everyone can follow along. The main purpose of this is for my own learning (I learn best when I can explain it to other people). In doing so, I hope that you readers will benefit greatly. If you see any mistake, be it ~~grammer~~ grammar mistake or conceptual mistake, please don't hesitate to let me know.



## Creating Regex

Therer are 2 ways to create Regex:

1. Regex literal. Example: `var re = /ab+c/`

2. Regex constructor (RegExp object). Example: `var re = new RegExp(‘ab+c’)`

## Regex Patterns:

Regex has **two** types of characters: simple characters and special characters.

Example: `/abc/` is simple and `/Chapter(\d+)\.\d*/` is special.

1. Simple patterns: `/abc/` looks for characters `‘abc’` that occur together in that order. It matches `“Hey, show me your abc’s”` or `“Evolution of slabcraft”`. `“Grab crab”` will not match because the space between `"ab c"`s

2. Special characters. For example: `/ab*c/` matches any character combination where single a is followed by 0 or more b’s (`*` means 0 or more). Will match `“cbbabbbbcdefg”` and `“abbc”`


Here are some list of special characters in regex ([source](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)). Don’t try to memorize them all. Know your limit. I can’t memorize them all in one day.

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

It matches preceding expression 1 or more times. Equivalent to {0,1}


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


If ? is used after *, +, ?, or {}, makes quantifier non-greedy. Just like the name implies, greedy means matching as many possible characters, while non-greedy means matching fewest possible characters.

For example, `/\d+/` will match one or more numbers. `\d` matches any decimal. `+` looks for 1 or more decimals.


```
var re = /\d+/
var str = “12345abcde”
str.match(re);          //=> [“12345”]

re = /\d+?/
str.match(re);          //=> [“1”]
```

`?` is also used in look-ahead assertions. Ex: x(?=y) and x(?!y). Explained later.



#### . (decimal point):

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


Those are the basics. We will get into better stuff next. For now, make sure you are comfortable with what we talked about here!
