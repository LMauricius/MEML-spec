# What is MEML?

Mauricio's Expandable Minimal Language.

It is a human friendly language for writing configuration files.
Inspired by TOML, with a more conscise, minimal feature set that allows for
simple domain-specific semantics without modifying the language's syntax.

It avoids redundancy and ambiguity, so there's always an obvious way to write data,
and the meaning is always clear as day.

# Features

MEML has everything you truly need for configuration and nothing more:
- `# Comments`
- Name-value pairs
- Numbers (Integer and Floating point)
- Strings (normal and raw)
- Lists
- Objects
- Tuples
- Literals (identifiers for keywords, referencing external variables, type notations, or any custom use-case)
- Adding values to existing lists and objects
- Binary, octal and hexadecimal numbers
- Scientific notation
- SI and Custom units
- Multiline, indented strings

# Example

This is an example of a record about users:

```meml
# This file contains some data...
# Edit it as needed!

Record title: "Users that visited us on this beautiful day"
Metadata: {
    Date: YMD 2025 2 14
    Time: HM 04 32 pm
}
Users: [
    {
        Name:            "John Smith"
        Weight:          75kg
        Distance:        5km
        Duration:        1h 6min 46s
        Math result:     6.02_+23
        Favourite color: rgb 65 127 240
        Scores: [
            9.8
            8.5
            9.1
        ]
        Remark: "
                 Was fun.
                 Would do it again.
                "
    }
    {
        Name:            "Alice Johnson"
        Weight:          60kg
        Distance:        8.3km
        Duration:        1h 45min 12s
        Math result:     3.14
        Favourite color: rgb 240 98 146
        Scores: [
            9.2
            8.9
            9.6
        ]
        Remark: "
                 Enjoyed the scenic route.
                 Planning to bring friends next time.
                "
    }
]
Today's mentioned trivia: {
    Mass of Earth: 5.97219_+24kg
    Tallest statue: "Statue of Unity"
    Most remote inhabited archipelago: "Tristan da Cunha"
}
```