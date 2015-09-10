<div align="center">
    <a href="http://lconf-data-serialization-format.github.io/">
        <img src="../../images/lconf-logo.png" alt="LCONF LOGO" title="The LCONF-Data-Serialization-Format Organization">
    </a>
</div>

# LCONF-Data-Serialization-Format-Standard

* 2015-09-09: First draft of the renamed and reversioned
    *LCONF-Data-Serialization-Format-Standard Documentation v0.1.0*.

* 2014-10-08: the last *LCONF* Version 7.0.0  was released by **peter1000**.

* In early 2014: **peter1000** <https://github.com/peter1000/> released a first *LCONF* specification and an
    implementation as python library.

## Copyrights & Licenses

The *LCONF-Data-Serialization-Format-Standard Documentation* and associated documentation
files (the "DOCUMENTATION") is licensed under the following terms:

> Copyright (c) 2014 - 2015, **peter1000**  <https://github.com/peter1000>.
>
> The DOCUMENTATION may be freely copied, published and distributed to
> others, provided that the above copyright notice and this Copyright
> License are included on all such copies or substantial portions of the
> DOCUMENTATION.
>
> However, the content of this DOCUMENTATION itself may not be modified
> in any way, including by removing the copyright notice, except as
> required to translate it into languages other than English or into a
> different format.
> In the event of discrepancies between a translated version and the
> official English version, the official English version shall govern.
>
> THIS DOCUMENTATION AND THE INFORMATION CONTAINED HEREIN IS PROVIDED
> "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING
> BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
> PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS
> OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
> LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
> ARISING FROM, OUT OF OR IN CONNECTION WITH THE DOCUMENTATION OR THE
> USE OF THE INFORMATION HEREIN.

# 1. LCONF Introduction

The LCONF-Data-Serialization-Format in short **LCONF** is a lightweight, text-based, data serialization format *with
emphasis on being human-friendly*.

LCONF builds upon concepts from [JSON](http://json.org/), [RSONLITE](https://pypi.python.org/pypi/rsonlite/0.1.0),
[INI](http://en.wikipedia.org/wiki/INI_file) and [YAML](http://yaml.org/).

LCONF defines a set of structuring rules with *indentation* and *selected characters* which provide structural
information. This excellent combination allows the data to show itself in a human-friendly, easily readable and
writeable format.

LCONF was specifically designed to be useful to people working with program configuration and configuration files as
well as for other common use cases such as data exchange and lightweight structural data storage.

LCONF uses LCONF-Schema-Definitions to descripe the structure and default content as well as any constraints on the
structure and content of a LCONF-Section, above and beyond the basic syntactical constraints imposed by LCONF itself.
LCONF-Schema-Definitions are valid LCONF syntax.

## 1.1. LCONF Preview

This section provides a quick preview into LCONF. It is not expected that the first-time reader understand all
of the examples but should be seen as motivation for the remainder of the specification.

Some of the examples are based on [YAML's spec preview examples](http://www.yaml.org/spec/1.2/spec.html#Preview).

### 1.1.1. Example: STRUCTURE_PAIR

Associates a LCONF-Key-Name with one data value - mapping key to value.

```text
name :: Tony Johnson
age :: 65
reg :: true
```

### 1.1.2. Example: STRUCTURE_LIST

Associates a LCONF-Key-Name with a list of data values - mapping key to sequence of values.

```text
- american
    Boston
    Detroit
    New York

# Compact form of a STRUCTURE_LIST
- national :: New York, Chicago, Atlanta
```

### 1.1.3. Example: STRUCTURE_SINGLE_BLOCK

A collection of any of the six LCONF-Structures.

```text
# Two STRUCTURE_SINGLE_BLOCKs.
. player1
    name :: Mark McGwire
    hr :: 65
    avg :: 0.278
. player2
    name :: Sammy Sosa
    hr :: 63
    avg :: 0.288
```

Example: Nested Mapping Of Mappings: LanguageSkills has three separate STRUCTURE_SINGLE_BLOCKs.

```text
. registry
    LastName :: Albert
    FirstName :: Fat
    Address :: 123 Cartoon Network Way, Hollywood, CA 12345
    . LanguageSkills
        . English
            Listening :: intermediate
            Speaking :: intermediate
            Reading :: good
            Writing :: basic
        . French
            Listening :: excellent
            Speaking :: native speaker
            Reading :: excellent
            Writing :: excellent
        . German
            Listening :: very good
            Speaking :: very good
            Reading :: excellent
            Writing :: good
```

### 1.1.4. Example: STRUCTURE_NAMED_BLOCKS

A collection of repeated named STRUCTURE_SINGLE_BLOCKs.

```text
# One STRUCTURE_NAMED_BLOCKS with two named STRUCTURE_SINGLE_BLOCKs.
* players
    . player1
        name :: Mark McGwire
        hr :: 65
        avg :: 0.278
    . player2
        name :: Sammy Sosa
        hr :: 63
        avg :: 0.288
```

Example: Nested Mapping Of Mappings: LanguageSkills usese one STRUCTURE_NAMED_BLOCKS with three named
STRUCTURE_SINGLE_BLOCKs.

```text
. registry
    LastName :: Albert
    FirstName :: Fat
    Address :: 123 Cartoon Network Way, Hollywood, CA 12345
    * LanguageSkills
        . English
            Listening :: intermediate
            Speaking :: intermediate
            Reading :: good
            Writing :: basic
        . French
            Listening :: excellent
            Speaking :: native speaker
            Reading :: excellent
            Writing :: excellent
        . German
            Listening :: very good
            Speaking :: very good
            Reading :: excellent
            Writing :: good
```

### 1.1.5. Example: STRUCTURE_UNNAMED_BLOCKS

A collection of repeated unnamed STRUCTURE_SINGLE_BLOCKs.

```text
* players
    .
        name :: Mark McGwire
        hr :: 65
        avg :: 0.278
    .
        name :: Sammy Sosa
        hr :: 63
        avg :: 0.288
```

### 1.1.6. Example: LCONF_SINGLE_BLOCK_REUSE

Assigns in a STRUCTURE_NAMED_BLOCKS sequence the settings of a previous STRUCTURE_SINGLE_BLOCK to a new
STRUCTURE_SINGLE_BLOCK.

```text
* Adresses
    . bill_to
        FirstName :: Mary
        LastName :: Watson
        Street :: 768 5th Ave # 1332
        City :: New York
        State :: NY
        ZIPCode :: 10019
        EmailAddress :: mary_watson@mail.go
        Phone :: +1 212-759-3000
    # LCONF_SINGLE_BLOCK_REUSE
    . ship_to == bill_to
        Street :: 175 E 62nd St APT 16A
        ZIPCode :: 10065
```

### 1.1.7. Example: STRUCTURE_TABLE

Associates a LCONF-Key-Name with tabular-data (columns and rows).

```text
| players
    | Mark McGwire | 65 | 0.278 |
    | Sammy Sosa   | 63 | 0.288 |
```

### 1.1.8. Example: Two LCONF-Sections In One LCONF-Text

```text
___SECTION :: 4 :: LCONF :: Ranking of 1998 home runs
- players
    Mark McGwire
    Sammy Sosa
    Ken Griffey
___END

___SECTION :: 4 :: LCONF :: Team ranking
- Ranking
    Chicago Cubs
    St Louis Cardinals
___END
```

### 1.1.9. Example: Full Length Example (Invoice)

```text
___SECTION :: 4 :: LCONF :: tag:clarkevans.com,2002:invoice

invoice :: 34843
date :: 2001-01-23
* Adresses
    . bill_to
        given :: Chris
        family :: Dumars
        . address
            lines: 458 Walkman Dr. \nSuite #292
            city :: Royal Oak
            state :: MI
            postal :: 48046
    . ship_to == bill_to
* product
    .
        sku :: BL394D
        quantity :: 4
        description :: Basketball
        price :: 450.00
    .
        sku :: BL4438H
        quantity :: 1
        description :: Super Hoop
        price :: 2392.00
tax :: 251.42
total :: 4443.52
# a list of comments which are part of the data
- comments
    Late afternoon is best.
    Backup contact is Nancy
    Billsmer @ 338-4338.
___END
```

## 1.2. LCONF Summary

### 1.2.1. Indentation

The indentation per Indentation-Level MUST be minimum 2 and maximum 8 spaces and MUST be specified in the
LCONF-Section-Start-Line.

### 1.2.2. LCONF_SECTION_FORMAT

LCONF uses LCONF-Schema-Definitions to descripe the structure and default content of a LCONF-Section.
In the LCONF-Section-Start-Line it MUST be specified if the LCONF-Section is a:

* `LCONF`: a regular LCONF-Section with data.
* `STRICT`: a special LCONF-Section which contains only LCONF-Schema-Definitions.
* `FLEXIBLE`: a special LCONF-Section which contains only LCONF-Schema-Definitions.

Examples

```text
___SECTION :: 4 :: LCONF :: Example Literal Name Tokens
registered :: true
___END
```

```text
___SECTION :: 4 :: STRICT :: Example Literal Name Tokens
. registered | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_BOOLEAN
    DEFAULT :: NOTSET
    EMPTY_REPLACEMENT :: NOTSET
___END
```

```text
___SECTION :: 4 :: FLEXIBLE :: Example Literal Name Tokens
. registered | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_BOOLEAN
    DEFAULT :: NOTSET
    EMPTY_REPLACEMENT :: NOTSET
___END
```

### 1.2.3. Literal Name Tokens

* LCONF_SECTION_START:  `___SECTION`
* LCONF_SECTION_END: `___END`
* LCONF_SECTION_FORMAT:  `LCONF`
* LCONF_SCHEMA_STRICT_FORMAT: `STRICT`
* LCONF_SCHEMA_FLEXIBLE_FORMAT: `FLEXIBLE`
* LCONF_TRUE: `true`
* LCONF_FALSE: `false`
* LCONF_NOTSET: `NOTSET`
* LCONF_FORCE: `FORCE`

```text
___SECTION :: 4 :: LCONF :: Example Literal Name Tokens
registered :: true
fluent_in_english :: false
weight :: NOTSET
interest_rate_rise :: 100.8|1.27|106|FORCE
___END
```

### 1.2.4. LCONF Structures

The set of six structures includes three simple structures and three collection structures.

The three simple structures are:

* STRUCTURE_PAIR
* STRUCTURE_LIST (inclusive Compact_STRUCTURE_LIST notation)
* STRUCTURE_TABLE

The three collection structures are:

* STRUCTURE_SINGLE_BLOCK
* STRUCTURE_NAMED_BLOCKS
* STRUCTURE_UNNAMED_BLOCKS

```text
___SECTION :: 4 :: LCONF :: Example The three simple structures
# STRUCTURE_PAIR
name :: Max

# STRUCTURE_LIST: STRUCTURE_LIST notation.
- color_name_list1
    Red
    Blue
    NOTSET
    Green

# STRUCTURE_LIST: Compact_STRUCTURE_LIST notation.
- color_name_list2 :: Red, Blue, NOTSET, Green

# STRUCTURE_TABLE: with a comment column names line
| people_table
    # name  | height_cm | weight_kg | age    |
    | Tim   | 178       | 86        | 37     |
    | Paula | 156       | NOTSET    | NOTSET |

___END
```

```text
___SECTION :: 4 :: LCONF :: Example The three collection structures
# STRUCTURE_SINGLE_BLOCK
. favorites
    food :: Spaghetti
    sport :: Soccer
    color :: Blue
    - color_name_list
        Red
        Blue

# STRUCTURE_NAMED_BLOCKS
* tests_named
    . test1
        score :: 90
        name :: One
    . test2
        score :: 96
        name :: Two

# STRUCTURE_UNNAMED_BLOCKS
* tests_unnamed
    .
        score :: 90
        name :: One
    .
        score :: 96
        name :: Two
___END
```

### 1.2.5. LCONF-Value-Types

The set of six main value types includes NOTSET, String, Boolean, Number, Date & Time and Range.

* **NOTSET**

    * TYPE_NOTSET: is the Literal Name Token `NOTSET` and is used to indicate the lack of a value and is different
        from an Empty-Value.

* **String**

    * TYPE_STRING: A sequence of zero or more Unicode characters.
    * TYPE_DIGITS: A TYPE_STRING constraint to only LCONF_DIGITS. `0-9`
    * TYPE_PATTERN_DIGITS: A TYPE_STRING constraint to a pattern where only the individual LCONF_DIGITS MAY change.
        `@@@-@@-@@@@`  could be used as pattern for `123-55-6678`.

* **Boolean**

    * TYPE_BOOLEAN: LCONF_TRUE or LCONF_FALSE.  Literal Name Token: `true` or `false`.

* **Number**

    * TYPE_INTEGER: MUST contain only LCONF_DIGITS. It MAY have a preceding LCONF_PLUS or LCONF_MINUS.
        64 bit (signed long) range expected (`-9223372036854775808` to `+9223372036854775807`).

        * LCONF_INTEGER_LOWEST  = -9223372036854775808
        * LCONF_INTEGER_HIGHEST = +9223372036854775807

    * TYPE_FLOAT: supports four different notations.

        * Fractional: `+3.1415`, `-3.1415`
        * Exponent: `5e+22`, `-2E-2`
        * Fractional And Exponent Mixed: `6.196E63`, `-1.54e-003`
        * Fraction P/Q Of Two Integers: `+3/4`, `-93/16`, `1/8`, `2789/-598`

    * TYPE_NUMBER: can be any of TYPE_INTEGER or TYPE_FLOAT.

* **Date & Time**

    * TYPE_MONTH: `YYYY-MM` e.g.: `1945-03`

    * TYPE_DAY: `YYYY-MM-DD` e.g. `2014-11-15`

    * TYPE_MINUTE: `hh:mm` e.g: `12:30`

    * TYPE_SECOND: `hh:mm:ss` e.g: `02:30:42`

    * TYPE_SECOND_FRACTION: `hh:mm:ss.fff` e.g: `12:30:59.001`, `04:02:00.000156`, `18:53:16.1`

    * TYPE_DAY_MINUTE1: `YYYY-MM-DD hh:mm` e.g: `2013-07-01 12:30`

    * TYPE_DAY_MINUTE2: `YYYY-MM-DDThh:mm` e.g: `2013-07-01T12:30`

    * TYPE_DAY_SECOND1: `YYYY-MM-DD hh:mm:ss` e.g: `2013-07-01 12:30:59`

    * TYPE_DAY_SECOND2: `YYYY-MM-DDThh:mm:ss` e.g: `2013-07-01T12:30:59`

    * TYPE_DAY_SECOND_FRACTION1: `YYYY-MM-DD hh:mm:ss.fff` e.g: `2013-07-01 04:02:00.000156`

    * TYPE_DAY_SECOND_FRACTION2: `YYYY-MM-DDThh:mm:ss.fff` e.g: `2013-07-01T12:30:59.001`

* **Range**

    A range defines an arithmetic sequence where the first element is the LCONF-Range-Start-Number.

    To force always the inclusion of the *LCONF-Range-End-Number* the Literal Name Token `FORCE` is set.

    * TYPE_RANGE_OF_ELEMENTS

        * Start-Number|Step-Number|Number-Of-Elements <br />
            Example: `-10|1|*21`, `512.4|0.125|*8`

    * TYPE_RANGE_BY_END_VALUE

        * Start-Number|Step-Number|End-Number <br />
            `100.8|1.27|106`
        * Start-Number|Step-Number|Number|FORCE <br />
            `100.8|1.27|106|FORCE`

```text
___SECTION :: 4 :: LCONF :: LCONF_Value_Types
# TYPE_NOTSET
key1 :: NOTSET

# TYPE_STRING
key2 :: Any text

# TYPE_DIGITS
registration_number :: 778945215643219945167845123689

# TYPE_PATTERN_DIGITS: `@@@-@@-@@@@`
social_security_number :: 123-55-6678

# TYPE_BOOLEAN
key3 :: true
key4 :: false

# TYPE_INTEGER
key5 :: +9223372036854775807
key6 :: -9223372036854775808

# TYPE_FLOAT
key7 :: -0.01
key8 :: 0.0
key9 :: +3.1415

key10 :: 5e+22
key11 :: -2E-2

key12 :: -1.54e-003
key13 :: 6.196E63

key14 :: 1/8
key15 :: +3/4

# TYPE_NUMBER
key16 :: +92
key17 :: -2E-2
key18 :: 1/100

# TYPE_MONTH
key19 :: 1932-08
key20 :: 2014-11

# TYPE_DAY
key19 :: 1932-08-27
key20 :: 2014-11-15

# TYPE_MINUTE
key21 :: 12:30

# TYPE_SECOND
key22 :: 02:30:42

# TYPE_SECOND_FRACTION
key23 :: 18:53:16.145

# TYPE_DAY_MINUTE1
key24 :: 2013-07-01 12:30

# TYPE_DAY_MINUTE2
key25 :: 2013-07-01T12:30

# TYPE_DAY_SECOND1
key26 :: 2013-07-01 12:30:59

# TYPE_DAY_SECOND2
key27:: 2013-07-01T12:30:59

# TYPE_DAY_SECOND_FRACTION1
key28 :: 2013-07-01 12:30:59.001

# TYPE_DAY_SECOND_FRACTION2
key29 :: 2013-07-01T12:30:59.001

# TYPE_RANGE_OF_ELEMENTS
# 21 elements: `-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10`
key29 :: -10|1|*21

# TYPE_RANGE_BY_END_VALUE
# elements: `-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5`
key30 :: -10|1|5

# elements: `100.8, 99.53, 98.26, 96.99, 95.72, 94.45, 93.18, 92.1`
key31 :: 100.8|-1.27|92.1|FORCE
___END
```

### 1.2.6. Summary of Restrictions

* LCONF-Sections MUST NOT contain any lines with *Trailing Space*.
* LCONF_BLANK_LINE: A line that contains only whitespace characters (zero or more) is not parsed.
* LCONF_SECTION_START Token `___SECTION` is a reserved LCONF character sequence.
* LCONF_SECTION_END Token `___END` is a reserved LCONF character sequence.
* TYPE_NOTSET `NOTSET` is a reserved LCONF character sequence.

#### 1.2.6.1. First None White Character Of A Line

Some first none white character of a line are **reserved** as special purpose *Identifiers*. Most of them are permitted
in values.

First None White Character Of A Line are reserved:

* LCONF_NUMBER_SIGN `#` is reserved only for `LCONF_COMMENT_LINE_IDENTIFIER`
* LCONF_SLASH `/` is reserved only for `LCONF_SCHEMA_COMMENT_LINE_IDENTIFIER`
* LCONF_VERTICAL_LINE `|` is reserved only for `STRUCTURE_TABLE_IDENTIFIER` and also used as
    `STRUCTURE_TABLE_VALUE_SEPARATOR`.
* LCONF_MINUS `-` is reserved only for `STRUCTURE_LIST_IDENTIFIER`.
* LCONF_PERIOD `.` is reserved only for `STRUCTURE_SINGLE_BLOCK_IDENTIFIER`.
* LCONF_ASTERISK `*` is reserved only for `STRUCTURE_NAMED_BLOCKS_IDENTIFIER` & `STRUCTURE_UNNAMED_BLOCKS_IDENTIFIER`.

#### 1.2.6.2. Unique LCONF-Key-Names

* Within a `STRUCTURE_SINGLE_BLOCK` all direct child LCONF-Key-Names (keys with one additional indentation level) MUST
    be unique.

* A `LCONF-Root` is a special STRUCTURE_SINGLE_BLOCK and all direct child LCONF-Key-Names (keys with no indentation
    level) MUST be unique.

* Within a `STRUCTURE_NAMED_BLOCKS` all direct child LCONF-Key-Names (keys with one additional indentation level) MUST
    be unique.

* LCONF-Column-Names (STRUCTURE_TABLE's Column-Names are also considered to be LCONF-Key-Names) MUST be unique within
    a STRUCTURE_TABLE.

#### 1.2.6.3. LCONF-Key-Names

The default LCONF_SCHEMA_STRICT_FORMAT `STRICT` adds some contraints to LCONF-Key-Names.

A LCONF_SCHEMA_STRICT_FORMAT LCONF-Key-Name (STRUCTURE_TABLE's LCONF-Column-Name are also considered to be
LCONF-Key-Names):

MUST be a sequence of one or more (but maximum thirty-one '31') characters of these groups:

* LCONF_UNDERSCORE
* LCONF_CAPITAL_LETTERS
* LCONF_SMALL_LETTERS
* LCONF_DIGITS

Additionally constraints:

* The first character MUST NOT be a LCONF_DIGITS
* The name SHOULD NOT be one of LCONF's Literal Name Tokens
* The name SHOULD NOT be one of common reserved programming words

### 1.2.7. LCONF-Schema-Definitions

LCONF uses LCONF-Schema-Definitions to descripe the structure and default content as well as any constraints on the
structure and content of a LCONF-Section, above and beyond the basic syntactical constraints imposed by LCONF itself.
LCONF-Schema-Definitions are valid LCONF syntax.

### 1.2.7.1. Full Length Example (Invoice)

A example LCONF-Schema for the: Full Length Example (Invoice)

```text
___SECTION :: 4 :: STRICT :: Schema: Full Length Example (Invoice)

. invoice | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_INTEGER

. date | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_DAY

. Adresses | STRUCTURE_NAMED_BLOCKS (1,2)

    . TEMPLATE_BLOCK | STRUCTURE_SINGLE_BLOCK

        . given | STRUCTURE_PAIR
            ITEM :: REQUIRED_NOT_EMPTY | TYPE_STRING

        . family | STRUCTURE_PAIR
            ITEM :: REQUIRED_NOT_EMPTY | TYPE_STRING

        . address | STRUCTURE_SINGLE_BLOCK

            . lines | STRUCTURE_PAIR
                ITEM :: REQUIRED_NOT_EMPTY | TYPE_STRING

            . city | STRUCTURE_PAIR
                ITEM :: REQUIRED_NOT_EMPTY | TYPE_STRING

            . state | STRUCTURE_PAIR
                ITEM :: REQUIRED_NOT_EMPTY | TYPE_STRING (2,2)

            . postal | STRUCTURE_PAIR
                ITEM :: REQUIRED_NOT_EMPTY | TYPE_INTEGER (10000,99999)

. product | STRUCTURE_UNNAMED_BLOCKS (1,NOTSET)

    . TEMPLATE_BLOCK | STRUCTURE_SINGLE_BLOCK

        . sku | STRUCTURE_PAIR
            ITEM :: REQUIRED_NOT_EMPTY | TYPE_STRING (6,6)

        . quantity | STRUCTURE_PAIR
            ITEM :: REQUIRED_NOT_EMPTY | TYPE_INTEGER (1,NOTSET)

        . description | STRUCTURE_PAIR
            ITEM :: REQUIRED_NOT_EMPTY | TYPE_STRING

        . price | STRUCTURE_PAIR
            ITEM :: REQUIRED_NOT_EMPTY | TYPE_FLOAT

. tax | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_FLOAT

. total | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_FLOAT

# This Comment line will not be parsed but
# the next one starting with a slah is a LCONF-Schema-Comment-Line and is parsed
/ a list of comments which are part of the data
. comments | STRUCTURE_LIST
    ITEM :: OPTIONAL | TYPE_STRING
___END
```
