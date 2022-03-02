# Vi commands

## Help System

### Commands

#### :help or :h

- > followed by other commands will display their usage  **Note:** using a caret symbol is the same as "Ctrl".

- ##### ^] and ^o

  - > search those words in help

- ##### ^g or :f(ile)

  - > shows file name

- ##### ^f & ^b

  - > similar to page down and page up

- ##### Ctrl-w-w

- > switching between tabs

## Yanking and Deleting

### Registers

- #### :reg displays registries and their content

  - > :reg fg will shows only contents of f and g registries

- #### numbered registers from *"0* to *"9*​ and symbols as well. Use uppercase registry to append to them

  - > "0p pastes latest yanked data even, when you used **dd** before.

  - > for using specific registry use " followed by character (i.e "jyy will yank a line and saves it in "j reg and "J append to it)

## Transforming and Substituting

### Transform

- #### i and I (uppercase i)

  - > uppercase inserts at **beginning** of line

- #### a and A (uppercase a)

  - > uppercase inserts at **end** of line

- #### J (uppercase j)

  - > concatenate current line and next ling with space between

- #### Replace Mode with R (uppercase r)

- #### c command like using d[motions] followed by i

- #### uppercase and lowercase

  - g~[motion] switch case (g~w switch case a word). ~ use for only a char
  - gU[motion] uppercase
  - gu[motion] lowercase

### Find and Substitute

- #### f vs t ---> f char included , t not included

  - > i.e assume "Hi! oh" --> df! results in " oh", however dt! makes it "! oh"

- #### use ; and , (comma) for next and prev appearance found by **f**

- #### :%s/old/new/g --> :[range]s/{pattern}/{string}/[flags] [count]

  - > % --> range means whole file could be *3,9* or *.,$* (from here to last line)
  - > g --> all occurrence

## Text Objects and Macros

### a vs i

-  using *a* will include obj despite what *i* does

### Word Objects
  
- > daw --> deletes word

### " or ' Objects

- > di" --> whatever between "

### [], (), {} and <> Objects

- > all same which included all texts in between them

### *\<tags\>* Objects

- all about **t** in dat or dit
- > put cursor on < tag > or < /tag > then type dat --> results in deleted block
- > or dit for just delelted tags inner text

### Macros

- JUST remember q[regesiter] to record (i.e. qa) and @[reg] to redo.  
- > Note: @@ redo last reg and 0 at begining of record
