# MEML syntax

This is a simple specification for the MEML configuration language.

# Values
There are 5 types of values: dictionaries, lists, numbers, strings and keywords:
- Dictionaries are sets of key-value pairs. Keys are strings, mapped to tuples. The file itself is a dictionary.
- Lists are ordered containers of tuples.

## Dictionaries
Dictionaries are enclosed in `{ }` and have fields named by identifiers
(containing all symbols except `:` and newline,
with the escape sequence `\` supporting any codepoint),
followed by `:` and a *tuple* of values.

Fields are put each into its own line.

The file itself is also a dictionary, just not enclosed by `{ }`.

## Lists
Lists are enclosed by `[ ]`. Each item starts on its own line, and is also a tuple.

## Numbers
Numbers can be both integers and decimal (with decimal dot `.` only).

Just for readability numbers can contain underscores between digits for spacing and grouping digits.
Example:
```meml
Long number: 1_000_000
```
Scientific notation is used by appending the exponent after the significand.
The exponent is appended after the last underscore `_`
and always has either the `+` or `-` sign.
Example:
```meml
Number in scientific notation: 6.02_+23
```

Binary, octal and hexadecimal numbers are also supported:
- prefix `0x` starts a hexadecimal number. Letters `a-f` and `A-F` are used for digits.
- prefix `0b` starts a binary number
- prefix `0o` starts an octal number.
  Just `0` as a prefix doesn't start an octal number, unlike common implementations.

Exponents in scientific notation are always written in decimal,
but use modify the number's significand in it's used base.

## Strings
Strings are enclosed by single or double quotes.

In single-quoted strings `"` character can be used unescaped,
and in double-quoted strings `'` can be used unescaped.
Escape sequence `\` can be used in normal strings for special characters, like newlines.

Raw strings can be multiline. They are written with the quote character followed by a new line.
Each line of the string needs to start indented by the same number of whitespaces as
characters before the opening quote, plus an additional space.
The closing quote is aligned to the opening quote, without an additional space.
All lines except the ones with opening and closing quotes are separate lines of the string,
ending with a newline character.
The lines are interpreted literally,
with quotes and `\` characters just being normal characters of the string.

## Keywords
Keywords are similar to strings, but not enclosed in quotes. They are still a separate type,
and usually used to denote the format of values, or to simplify writing of one-worded text.
They start with a non-digit and can contain any character except the special ones used in values, `( ) [ ] { } " '` or newlines and spaces. For those characters you can use the escape sequence `\`.

## Tuples
The tuples are lists of values separated by a space ` `. All values are contained in tuples.
It can be of any size, and contain values of any types.
The space separating values is always required,
even before special characters starting some values, like `[ ] { } " '`

End of the line usually signifies the end of the tuple,
but the escape character `\` at the line end can be used to continue the tuple on the next line.

Example:
```meml
I'm a multiline tuple: 1 two \
    3.0 0b1_+2 "five"
```

# Escape sequences
Escape sequences are used to write special characters not directly allowed in strings,
identifiers and keywords, or those that are hard to type.
The characters that are part of the escape sequence are never interpreted as special MEML symbols.
They start with a backslash `\` and can be followed by:
- `n` - newline character
- `_` - normal whitespace
- `:` - colon
- `'` - apostrophe or single quote (not interpreted as string begin/end)
- `"` - double quote (not interpreted as string begin/end)
- `(` - opening parenthesis
- `)` - closing parenthesis
- `[` - opening bracket
- `]` - closing bracket
- `{` - opening brace
- `}` - closing brace
- `t` - tab
- `v` - vertical tab
- `r` - carriage return
- `b` - backspace
- `a` - alert (bell, beep), used in some terminals
- `f` - formfeed page break
- `e` - escape character symbol, used in some renderers and terminals
- `0` - Null character. While supported, using it in MEML is discouraged
- `x` and 2 hex digits - The byte of the specified hexadecimal value
- `u` and 4 hex digits - Unicode code point of the specified hexadecimal value below 0x10000 (65536)
- `U` and 8 hex digits - Unicode code point of the specified hexadecimal value

While some of these characters are supported in some places like in identifiers, they can still be written using escape sequences.

In raw (multiline) strings all characters are interpreted literally until the end of the line.
That means you can't use escape sequences in those strings,
but you can write any character or byte sequence as it is.