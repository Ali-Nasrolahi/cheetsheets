# Vi commands

- [Vi commands](#vi-commands)
  - [Help System](#help-system)
    - [Commands](#commands)
  - [Yanking and Deleting](#yanking-and-deleting)
    - [Registers](#registers)
  - [Transforming and Substituting](#transforming-and-substituting)
    - [Transform](#transform)
    - [Find and Substitute](#find-and-substitute)
  - [Text Objects and Macros](#text-objects-and-macros)
    - [a vs i](#a-vs-i)
    - [Word Objects](#word-objects)
    - [" or ' Objects](#-or--objects)
    - [[], (), {} and <> Objects](#---and--objects)
    - [*\<tags\>* Objects](#tags-objects)
    - [Macros](#macros)
  - [Buffers](#buffers)
    - [Display buffers with :buffer, :ls or :files](#display-buffers-with-buffer-ls-or-files)
    - [Switching buffers](#switching-buffers)
    - [Add and Delete buffers](#add-and-delete-buffers)
  - [Vim split Windows](#vim-split-windows)
    - [Splitting](#splitting)
    - [Navigating](#navigating)
    - [Resize](#resize)

## Help System

### Commands

`:help or :h`

followed by other commands will display their usage.

**Note:** using a caret symbol is the same as "Ctrl".

`^] and ^o`

- search those words in help

`^g or :f(ile)`

- shows file name

`^f & ^b`

- similar to page down and page up

## Yanking and Deleting

### Registers

`:reg` displays registries and their content

- `:reg fg` will shows only contents of f and g registries

numbered registers from *"0* to *"9*​ and symbols as well. Use uppercase registry to append to them

- `"0p` pastes latest yanked data even, when you used **dd** before.

- for using specific registry use `"` followed by character (i.e `"jyy` will yank a line and saves it in `"j` reg and `"J` **append** to it)

## Transforming and Substituting

### Transform

`i and I`

- uppercase inserts at **beginning** of line and lowercase at current position

`a and A`

- uppercase inserts at **end** of line and lowercase inserts at next char

`J`

- concatenate current line and next ling with space between

Replace Mode with `R`

`c` command like using d[motions] followed by i

uppercase and lowercase

- `g~[motion]` switch case (g~w switch case a word). ~ use for only a char
- `gU[motion]` uppercase
- `gu[motion]` lowercase

### Find and Substitute

`f` vs `t` ---> f char included , t not included

- > i.e assume "Hi! oh" --> `df!` results in " oh", however dt! makes it "! oh"

use `;` and `,` for next and prev appearance found by **f**

```vim
:%s/old/new/g --> :[range]s/{pattern}/{string}/[flags] [count]
```

- `%` --> range means whole file could be *3,9* or *.,$* (from here to last line)
- `g` --> all occurrence
`

## Text Objects and Macros

### a vs i

using `a` will include obj despite what `i` does

### Word Objects
  
- `daw` --> deletes word

### " or ' Objects

- `di"` --> whatever between "

### [], (), {} and <> Objects

- all same which included all texts in between them

### *\<tags\>* Objects

It's all about **t** in `dat` or `dit`

- > put cursor on < tag > or < /tag > then type dat --> results in deleted block

- > or dit for just deleted tags inner text

### Macros

JUST remember `q[register]` to record (i.e. qa) and `@[reg]` to redo.  

- > Note: `@@` redo last reg and 0 at beginning of record

## Buffers

### Display buffers with :buffer, :ls or :files

### Switching buffers

- `:b[id]` ,changes to N buffer
- `:b [name]` ,changes to buffer associated with [name] file
- `:bp` and `:bn` for previous and next buffer
- `:bf` and `:bl` for first and last buffer

### Add and Delete buffers

- `:bd([id])` --> (i.e. bd or bd2)
- `:e [name of file]`
- `:E opens Explorer`

To execute a command in all buffers use :bufdo

```vim
:bufdo %s/old/new/g " --> replaces all old with new
```

To save or discard changes in all buffers use `:qall!` or `:wall`

## Vim split Windows

### Splitting

- `^w + v` vertical split
- `^w + s` horizontal split

### Navigating

- `^w + (h/j/k/l)` to move around windows

### Resize

- `^w + (shift + '>')` increase or
- `^w + (shift + '<')` decrease width
- `^w + (shift + '+')` increase or
- `^w + -` decrease height
- `^w + =` splitting in equally windows
- `^w + |` kind of zoom in panes , use `^w + =` to undo
- `^w + r or R` Rotate windows
