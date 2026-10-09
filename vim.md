# `vim`

A practical reference for help, editing, text objects, registers, macros, buffers, and split
windows. Commands assume Vim; some features may differ in traditional `vi`.

- [`vim`](#vim)
  - [Help system](#help-system)
  - [Yanking, deleting, and registers](#yanking-deleting-and-registers)
    - [Common operations](#common-operations)
    - [Registers](#registers)
  - [Inserting and transforming text](#inserting-and-transforming-text)
    - [Insert and replace](#insert-and-replace)
    - [Change, case, and replacement](#change-case-and-replacement)
  - [Find and substitute](#find-and-substitute)
    - [Character search](#character-search)
    - [Search and substitute](#search-and-substitute)
  - [Text objects](#text-objects)
    - [`a` vs. `i`](#a-vs-i)
    - [Word objects](#word-objects)
    - [Quotes and delimiters](#quotes-and-delimiters)
    - [Tags](#tags)
  - [Macros](#macros)
  - [Buffers](#buffers)
    - [List and inspect buffers](#list-and-inspect-buffers)
    - [Switch buffers](#switch-buffers)
    - [Open and delete buffers](#open-and-delete-buffers)
    - [Execute commands across buffers](#execute-commands-across-buffers)
    - [Save or quit across buffers](#save-or-quit-across-buffers)
  - [Split windows](#split-windows)
    - [Split](#split)
    - [Navigate between windows](#navigate-between-windows)
    - [Resize and rearrange](#resize-and-rearrange)
  - [Quick reminders](#quick-reminders)

## Help system

```vim
:help                  " Open help
:help <topic>          " Search help for a topic
:help :substitute      " Help for an Ex command
:help i_CTRL-W         " Help for a specific insert-mode key
```

Useful navigation inside help:

- `Ctrl-]` — follow the help tag under the cursor.
- `Ctrl-O` — jump back in the jump list.
- `Ctrl-I` — jump forward in the jump list.
- `Ctrl-F` / `Ctrl-B` — page forward / backward.
- `Ctrl-G` or `:file` — show the current file name and status.

Use `:help index` to browse key mappings and `:help quickref` for Vim's built-in quick reference.

## Yanking, deleting, and registers

### Common operations

```vim
yy          " Yank (copy) the current line
yw          " Yank a word forward
p           " Put after the cursor / below the line
P           " Put before the cursor / above the line
dd          " Delete the current line
dw          " Delete from cursor through a word motion
d$          " Delete to end of line
D           " Delete to end of line
x           " Delete character under cursor
X           " Delete character before cursor
```

Vim's operators combine with motions: `d` deletes, `y` yanks, and `c` changes text. For example, `d}` deletes to the next paragraph boundary, while `c$` changes to the end of the line.

### Registers

```vim
:registers           " List registers and their contents
:reg a               " Inspect register a
"ayy                 " Yank the current line into register a
"ap                  " Put the contents of register a
"Ayy                 " Append the line to register a
```

Use `"` followed by a register name to select a register. Uppercase names append to the corresponding lowercase register.

Useful registers:

- `"0` — most recent yank.
- `"1`–`"9` — numbered delete history; large deletes shift through these registers.
- `"-` — most recent small delete.
- `"+` — system clipboard, when clipboard support is available.
- `"*` — selection clipboard on systems that support it.
- `"/` — last search pattern.
- `":` — most recent command-line command.
- `"%` — current file name.

Example: `dd` followed by `"0p` pastes the most recently yanked text rather than the deleted line.

## Inserting and transforming text

### Insert and replace

```vim
i           " Insert before the cursor
I           " Insert at the first non-blank character of the line
a           " Append after the cursor
A           " Append at the end of the line
o           " Open a new line below
O           " Open a new line above
R           " Enter Replace mode
J           " Join the current line with the next line
```

### Change, case, and replacement

```vim
cw          " Change from cursor through a word motion
cc          " Change the entire line
C           " Change to the end of the line

g~w         " Toggle case over a word motion
gUw         " Uppercase over a word motion
guw         " Lowercase over a word motion
~           " Toggle case of the character under the cursor
```

Operators such as `d`, `c`, `y`, `gU`, and `gu` can be combined with motions or text objects. For example, `gUiw` uppercases the word under the cursor.

## Find and substitute

### Character search

```vim
f<char>     " Find character forward, including the character
t<char>     " Move forward until just before the character
F<char>     " Find character backward
T<char>     " Move backward until just after the character
;           " Repeat the last f/F/t/T search in the same direction
,           " Repeat it in the opposite direction
```

Example, with the cursor at the beginning of `Hi! oh`:

- `df!` deletes through `!`, leaving `oh`.
- `dt!` deletes up to but not including `!`, leaving `! oh`.

### Search and substitute

```vim
/pattern                    " Search forward
?pattern                    " Search backward
n                           " Repeat search in the same direction
N                           " Repeat search in the opposite direction

:s/old/new/                 " Replace first match on current line
:s/old/new/g                " Replace all matches on current line
:%s/old/new/g               " Replace all matches in the file
:%s/old/new/gc              " Confirm each replacement
:3,9s/old/new/g             " Replace across lines 3–9
:.,$s/old/new/g             " Replace from current line to end of file
```

General form:

```vim
:[range]s/{pattern}/{replacement}/[flags]
```

- `%` — entire file.
- `.` — current line.
- `$` — last line.
- `g` — all matches on each selected line, rather than only the first.
- `c` — confirm each replacement.
- `i` — case-insensitive matching.

For literal patterns, remember that Vim uses its own regular-expression syntax. See `:help pattern` and `:help :substitute`.

## Text objects

Text objects let you operate on a logical unit of text rather than manually selecting a range. Combine them with operators such as `d`, `c`, and `y`.

### `a` vs. `i`

- `i` — inner text, excluding the surrounding delimiters.
- `a` — a text object including its surrounding delimiters or whitespace, depending on the object.

For example, inside `"hello"`:

```vim
di"         " Delete hello, keeping the quotes
da"         " Delete hello and the quotes
ci(         " Change the contents inside parentheses
ya{         " Yank a brace-delimited block
```

### Word objects

```vim
iw          " Inner word
aw          " A word, including surrounding whitespace where applicable
daw         " Delete a word
ciw         " Change a word
viw         " Select a word
```

`w` means a word made of letters, digits, and certain other characters; `W` treats a sequence separated by whitespace as a WORD. For example, `ciW` changes the whole whitespace-delimited token.

### Quotes and delimiters

```vim
di"         " Delete inside double quotes
ci'         " Change inside single quotes
da(         " Delete around parentheses
ci[         " Change inside square brackets
ya{         " Yank around braces
```

Common delimiter text objects include:

- `(` or `)` — parentheses.
- `[` or `]` — square brackets.
- `{` or `}` — braces.
- `<` or `>` — angle brackets.

Use the delimiter relevant to the text. Support for angle brackets and other object details can depend on the specific text object and Vim configuration.

### Tags

Vim supports tag text objects for markup such as HTML and XML.

```vim
dit         " Delete inside a tag
dat         " Delete around a tag, including its tag delimiters
cit         " Change inside a tag
vat         " Select around a tag
```

For example, with the cursor inside `<p>Hello</p>`, `dit` removes `Hello`, while `dat` removes the tag block as well.

## Macros

Macros record a sequence of keystrokes into a register so it can be replayed.

```vim
qa          " Start recording into register a
q           " Stop recording
@a          " Execute macro a
@@          " Repeat the last executed macro
10@a        " Execute macro a ten times
```

A useful pattern is to record a repeatable edit on one line, then replay it on similar lines. Test the macro on a small number of lines before applying it broadly.

## Buffers

A buffer is Vim's in-memory representation of a file or editing content. It is not the same thing as a window: multiple windows can display the same buffer.

### List and inspect buffers

```vim
:ls         " List buffers
:buffers    " List buffers
:files      " List buffers
```

### Switch buffers

```vim
:b 3            " Switch to buffer number 3
:buffer file.c  " Switch to a buffer matching the file name
:bn             " Next buffer
:bp             " Previous buffer
:bf             " First buffer
:bl             " Last buffer
```

### Open and delete buffers

```vim
:e file.c       " Edit/open a file
:bd             " Delete the current buffer
:bd 3           " Delete buffer 3
:bd!            " Delete current buffer, discarding its unsaved changes
:Explore        " Open Vim's built-in file explorer, when available
```

Deleting a buffer does not necessarily close every window; a window displaying it may switch to another buffer.

### Execute commands across buffers

```vim
:bufdo %s/old/new/ge
:bufdo update
```

The first command runs the substitution in each buffer; `e` suppresses errors when a buffer has no match. The second writes modified buffers. Review the affected files before saving bulk edits.

### Save or quit across buffers

```vim
:wall       " Write all modified buffers
:qall       " Quit all windows if there are no unsaved changes
:qall!      " Quit all and discard unsaved changes
:wqall      " Write modified buffers and quit
```

Be careful with `:qall!`, especially after bulk edits.

## Split windows

### Split

```vim
:split file.c       " Horizontal split, optionally opening a file
:vsplit file.c      " Vertical split, optionally opening a file
Ctrl-W s            " Horizontal split
Ctrl-W v            " Vertical split
```

### Navigate between windows

```vim
Ctrl-W h            " Move to the window on the left
Ctrl-W j            " Move to the window below
Ctrl-W k            " Move to the window above
Ctrl-W l            " Move to the window on the right
Ctrl-W w            " Move to the next window
Ctrl-W p            " Move to the previous window
```

### Resize and rearrange

```vim
Ctrl-W >            " Increase window width
Ctrl-W <            " Decrease window width
Ctrl-W +            " Increase window height
Ctrl-W -            " Decrease window height
Ctrl-W =            " Equalize window sizes
Ctrl-W |            " Maximize current window width
Ctrl-W _            " Maximize current window height
Ctrl-W r            " Rotate windows downward/rightward
Ctrl-W R            " Rotate windows upward/leftward
```

The number of columns or lines changed by a resize command can be specified before the command, for example `3 Ctrl-W >`.

## Quick reminders

| Goal | Command |
| --- | --- |
| Find help | `:help <topic>` |
| Copy a line | `yy` |
| Delete a line | `dd` |
| Paste a specific register | `"ap` |
| Change a word | `ciw` |
| Delete inside quotes | `di"` |
| Replace throughout a file | `:%s/old/new/g` |
| Record and replay edits | `qa` … `q`, then `@a` |
| List buffers | `:ls` |
| Switch buffers | `:b <number>` |
| Open a vertical split | `Ctrl-W v` |
| Move between splits | `Ctrl-W h/j/k/l` |
| Equalize split sizes | `Ctrl-W =` |

For more detail, see `:help registers`, `:help text-objects`, `:help usr_10`, `:help buffers`, and `:help windows`.
