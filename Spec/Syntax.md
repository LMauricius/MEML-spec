# MEML syntax

This is a simple specification for the MEML configuration language.

# Values
There are 5 types of values: dictionaries, lists, numbers, strings and keywords.

## Dictionaries
Dictionaries are sets of key-value pairs, called **fields**, enclosed by `{ }`.
Each field is named by an **identifier**
(containing all symbols except `:` and newline,
with the escape sequence `\` supporting any codepoint),
followed by `:` and a *tuple* of **values**.
Example:
```meml
Today's mentioned trivia: {
    Mass of Earth: 5.97219_+24kg
    Tallest statue: "Statue of Unity"
    Most remote inhabited archipelago: "Tristan da Cunha"
}
```

Fields are put each into its own line. Tuples containing one value are still tuples
and they can contain no values at all too.

Whitespace around the identifiers is trimmed,
so if you *really* need an identifier to start or end with space
use the escape sequence `\_`.

The file itself is also a dictionary, just not enclosed by `{ }`.

## Lists
Lists are series of **items** enclosed by `[ ]`.
Each item starts on its own line, and is also a tuple.
Example:
```meml
Scores: [
    9.8
    8.5
    9.1
]
```

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
Example:
```meml
Binary sequence: 0b10_1100_0101
```

Exponents in scientific notation are always written in decimal,
but use modify the number's significand in it's used base.
```meml
Big binary: 0b1_+9 # Equal to 0b10_0000_0000
```

Numbers can have suffixes, usually used as units.
The suffix starts with the first symbol that isn't otherwise expected at that part of the number
and  can contain any character except the special ones used in values,
`( ) [ ] { } " '` or newlines and spaces. For those characters you can use the escape sequence `\`.
Example:
```meml
Weight: 75kg
Distance: 5km
```

## Strings
Strings are enclosed by single or double quotes.
Example:
```meml
Name: "John Smith"
```

In single-quoted strings `"` character can be used unescaped,
and in double-quoted strings `'` can be used unescaped.
Escape sequence `\` can be used in normal strings for special characters, like newlines.
Example:
```meml
Line: "John said \"Hello world!\""
```

Raw strings can be multiline. They are written with the quote character followed by a new line.
Each line of the string needs to start indented by the same number of whitespaces as
characters before the opening quote, plus an additional space.
The closing quote is aligned to the opening quote, without an additional space.
All lines except the ones with opening and closing quotes are separate lines of the string,
ending with a newline character.
The lines are interpreted literally,
with quotes and `\` characters just being normal characters of the string.
Example:
```meml
Comment: "
          Enjoyed the scenic route.
          Planning to bring friends next time.
         "
```

Note: due to some editors making it harder to control indentation
enough that the string lines consistently align with the opening quote on a busy line,
it is recommended to put raw string opening quotes on their own lines.
Example:
```meml
Comment: \
    "
     Enjoyed the scenic route.
     Planning to bring friends next time.
    "
```

## Keywords
Keywords are similar to strings, but not enclosed in quotes. They are still a separate type,
and usually used to denote the format of values, or to simplify writing of one-worded text.
They start with a non-digit and can contain any character except the special ones used in values,
`( ) [ ] { } " '` or newlines and spaces. For those characters you can use the escape sequence `\`.
Example:
```meml
Blood type: AB+
```

## Tuples
The tuples are lists of values separated by a space ` `. All values are contained in tuples.
It can be of any size, and contain values of any types.
The space separating values is always required,
even before special characters starting some values, like `[ ] { } " '`
Example:
```meml
Favourite color: rgb 240 98 146
```

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

While some of these characters are supported in some places like in identifiers,
they can still be written using escape sequences.

In raw (multiline) strings all characters are interpreted literally until the end of the line.
That means you can't use escape sequences in those strings,
but you can write any character or byte sequence as it is.