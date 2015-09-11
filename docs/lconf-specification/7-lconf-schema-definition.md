<div align="center">
    <a href="http://lconf-data-serialization-format.github.io/">
        <img src="../../images/lconf-logo.png" alt="LCONF LOGO" title="The LCONF-Data-Serialization-Format Organization">
    </a>
</div>

> This file is part of the *LCONF-Data-Serialization-Format-Standard Documentation*.
>
> Copyright (c) 2014 - 2015, **peter1000**  <https://github.com/peter1000>.

# 7. LCONF-Schema-Definitions

LCONF uses LCONF-Schema-Definitions to descripe the structure and default content as well as any constraints on the
structure and content of a LCONF-Section, above and beyond the basic syntactical constraints imposed by LCONF itself.
LCONF-Schema-Definitions are valid LCONF syntax.

## 7.1. LCONF-Schema-Formats

There are two supported LCONF-Schema-Formats:

* LCONF_FORMAT_SCHEMA_STRICT:   `STRICT`   (uses additional constraints: e.g. naming conventions)
* LCONF_FORMAT_SCHEMA_FLEXIBLE: `FLEXIBLE` (less constraints)

All LCONF-Libraries MUST support the *LCONF_FORMAT_SCHEMA_STRICT* and MAY implement individual *LCONF_FORMAT_SCHEMA_FLEXIBLE*.

All LCONF-Schema-Definition Sections MUST set the used LCONF-Schema-Format in the: LCONF-Section Start Line:

LCONF-Schema-Definition Section-Start-Line MUST

* start with the LCONF_SECTION_START token
* followed by one LCONF_SPACE
* followed by one LCONF_KEY_VALUE_SEPARATOR
* followed by one LCONF_SPACE
* followed by a LCONF-Indentation-Per-Level
* followed by one LCONF_SPACE
* followed by one LCONF_KEY_VALUE_SEPARATOR
* followed by one LCONF_SPACE
* followed be a LCONF_FORMAT_SCHEMA_STRICT or LCONF_FORMAT_SCHEMA_FLEXIBLE
* followed by one LCONF_SPACE
* followed by one LCONF_KEY_VALUE_SEPARATOR
* followed by one LCONF_SPACE
* followed by a LCONF-Section-Name

```text
___SECTION :: 4 :: STRICT :: Menu Configuration Schema Definition`
```

```text
___SECTION :: 4 :: FLEXIBLE :: Menu Configuration Schema Definition`
```

### 7.1. 1. LCONF_FORMAT_SCHEMA_STRICT

This is the default LCONF-Schema-Format and MUST be supported by all LCONF-Libraries.

**Naming Convention**

A LCONF_FORMAT_SCHEMA_STRICT LCONF-Key-Name (STRUCTURE_TABLE's LCONF-Column-Name are also considered to be
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

### 7.1.2. LCONF_FORMAT_SCHEMA_FLEXIBLE

This LCONF-Schema-Format has fewer constraints but MAY NOT be supported by all LCONF-Libraries.

**Naming Convention**

A LCONF_FORMAT_SCHEMA_FLEXIBLE LCONF-Key-Name (STRUCTURE_TABLE's LCONF-Column-Name are also considered to be
LCONF-Key-Names) is a sequence of one or more Unicode characters and is case-sensitive. It MUST NOT contain any
LCONF_VERTICAL_LINE `|`.

## 7.2. Schema: *IDENTIFIER* Line

Each LCONF-Schema-Definition is a *STRUCTURE_SINGLE_BLOCK* where the Identifier-Line LCONF-Key-Name:

A LCONF-Schema-Identifier-Line MUST

* start with a STRUCTURE_SINGLE_BLOCK_IDENTIFIER
* followed by one LCONF_SPACE
* followed by a LCONF-Key-Name which MUST be the same as the LCONF-Key-Name in the final LCONF-Section it describes
* followed by one LCONF_SPACE
* followed by one LCONF_SCHEMA_SEPARATOR
* followed by one LCONF_SPACE
* followed by a LCONF-Structure for the LCONF-Key-Name in the final LCONF-Section it describes
* and optionally followed by one LCONF_SPACE and an additional LCONF-Constrain option

Example LCONF-Schema Identifier-Lines

```text
. Registered | STRUCTURE_PAIR

. Colors | STRUCTURE_LIST

. mobile_numbers | STRUCTURE_LIST (0,3)

. mobile_numbers | STRUCTURE_LIST (0,3)

. credit_cards | STRUCTURE_UNNAMED_BLOCKS (1,2)
```

Example of a corresponding LCONF-Section

```text
Registered :: true

- Colors
    Blue
    Red

# minimum required list items 0, maximum allowed items 3
- mobile_numbers

# minimum required blocks 1, maximum allowed blocks 2
* credit_cards
    .
        issuing_network :: Visa
        card_number :: 4639588964759877
```

### 7.2.1. Additional Constraints

Some elements can have **one** additional constraint after the LCONF-Structure.

* STRUCTURE_LIST
* STRUCTURE_TABLE,
* STRUCTURE_NAMED_BLOCKS
* STRUCTURE_UNNAMED_BLOCKS

can set a minimum/maximum number of elements they require. (LCONF-Constrain-Min-Max)

A LCONF-Constrain-Min-Max MUST:

* start with a LCONF_LEFT_PARENTHESIS
* followed by a *LCONF-Constrain-Min*: TYPE_INTEGER greater than zero (0)
* followed by a LCONF_COMMA
* followed by a *LCONF-Constrain-Max*: TYPE_INTEGER greater than zero (0)
* followed by a LCONF_RIGHT_PARENTHESIS

IMPORTANT: To not define one of the two constraints set it to TYPE_NOTSET (LCONF_NOTSET).

```text
# minimum required list items 2, maximum allowed items 5
. MobileNumbers | STRUCTURE_LIST (2,5)

# minimum required table rows not restricted, maximum allowed required table rows 5
. interests | STRUCTURE_TABLE (NOTSET,5)

# minimum required blocks 1, maximum allowed blocks not restricted
. credit_cards | STRUCTURE_UNNAMED_BLOCKS (1,NOTSET)
```

## 7.3. Schema: *ITEM* Pair

This is required except STRUCTURE_SINGLE_BLOCK, STRUCTURE_NAMED_BLOCKS, STRUCTURE_UNNAMED_BLOCKS only use the
*LCONF-Schema Identifier-Line*.

The Value for this pair is a LCONF-Item-Requirement-Option followed by a LCONF-Value-Type.

**EXCEPTION:** STRUCTURE_TABLE uses only the LCONF-Item-Requirement-Option because LCONF-Value-Types are set per
Column.

A LCONF-Schema *ITEM* Pair MUST

* start with a LCONF-Key-Name: `ITEM` (U+0049 U+0054 U+0045 U+004D)
* followed by one LCONF_SPACE
* followed by one LCONF_KEY_VALUE_SEPARATOR
* followed by one LCONF_SPACE
* followed by one of the LCONF-Item-Requirement-Option
* followed by one LCONF_SPACE
* followed by one LCONF_SCHEMA_SEPARATOR
* followed by one LCONF_SPACE
* followed by a LCONF-Value-Type for the LCONF-Key-Name in the final LCONF-Section it describes
* and optionally followed by one LCONF_SPACE and an additional LCONF-Constrain option

    * Exception: for TYPE_PATTERN_DIGITS this is not optional but a pattern MUST be defined

Example LCONF-Schema *ITEM* Pair

```text
. Colors | STRUCTURE_LIST
    ITEM :: OPTIONAL | TYPE_STRING

. Sex | STRUCTURE_PAIR
    ITEM :: REQUIRED | TYPE_STRING [Male, Female]

. ports | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_INTEGER [25,80,443]

. credit_card_number | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_DIGITS (15,19)

. telephone_number | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_PATTERN_DIGITS (+@ @@@ @@@ @@@@)

. interest_rate | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_FLOAT (0.5,<10.0)

. interest_rate2 | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_FLOAT (>1.0,10.0)
```

Example of a corresponding LCONF-Section

```text
Registered :: true

- Colors
    Blue
    Red

# predefined valid values one of `Male`,` Female`
Sex :: Female

# predefined valid values one of `25`, `80`, `443`
ports :: 80

# minimum required chars (digits) 15, maximum allowed  chars (digits) 19
credit_card_number :: 4639588964759877

# Pattern (+@ @@@ @@@ @@@@)
telephone_number :: +1 212 748 4496

# minimum required value 0.5, maximum allowed value less than 10.0
interest_rate :: 9.9999

# minimum required value greater than 1.0, maximum allowed value 10.0
interest_rate2 :: 10.0
```

### 7.3.1 LCONF-Item-Requirement-Option

* OPTIONAL

    * Item is NOT REQUIRED to be defined in a LCONF-Section
    * Item COULD be defined but set `NOTSET`
    * Item COULD be defined and set empty (which will overwrite any defaults)
    * Item COULD be defined and have set a value in accordance to the expected LCONF-Value-Type

* REQUIRED (this CAN be an Empty-Value)

    * Item MUST be defined in a LCONF-Section and MUST NOT be set `NOTSET`
    * Item MUST be defined and can be set empty (which will overwrite any defaults)
    * Item MUST be defined and can set a value in accordance to the expected LCONF-Value-Type

* REQUIRED_NOT_EMPTY (this MUST NOT be an Empty-Value)

    * Item MUST be defined in a LCONF-Section and MUST NOT be set `NOTSET` and MUST NOT be set empty
    * Item MUST be defined and MUST set a value in accordance to the expected LCONF-Value-Type

**EXCEPTION:** STRUCTURE_TABLE uses only the Item-Requirement-Option because LCONF-Value-Types are set per Column.

### 7.3.2. Additional Constraints

Some elements can have **one** additional constraint after the LCONF-Value-Type.

#### 7.3.1.1. List of Predefined Valid Values

For all LCONF-Value-Types one can define optional a list of predefined values (LCONF-Constrain-Predefined-Values).

A LCONF-Constrain-Predefined-Values MUST:

* start with a LCONF_LEFT_SQUARE_BRACKET
* followed by one or more *valid Values* separated by LCONF_COMMA
* followed by a LCONF_RIGHT_SQUARE_BRACKET

NOTE: LCONF_NOTSET and if an Empty-Value is valid is set in the LCONF-Item-Requirement-Option part.

```text
. Sex | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_STRING [Male, Female]

. Sex2 | STRUCTURE_PAIR
    ITEM :: REQUIRED | TYPE_STRING [Male, Female]

. ports | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_INTEGER [25,80,443]
```

Example of a corresponding LCONF-Section

```text
# predefined valid values one of `Male`,` Female`
Sex :: Female

# predefined valid values one of `Male`,` Female`
#   can also be empty
Sex ::

# predefined valid values one of `25`, `80`, `443`
ports :: 80
```

#### 7.3.1.2. Digits Pattern

For TYPE_PATTERN_DIGITS one MUST define either a list of predefined valid values (LCONF-Constrain-Predefined-Values) or
a LCONF-Constrain-Digits-Pattern which MUST:

* start with a LCONF_LEFT_PARENTHESIS
* followed by the required pattern (`@` are placeholders for digits)
* followed by a LCONF_RIGHT_PARENTHESIS

NOTE: LCONF_NOTSET and if an Empty-Value is valid is set in the LCONF-Item-Requirement-Option part.

Example LCONF-Schema *ITEM* Pair

```text
. telephone_number | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_PATTERN_DIGITS (+@ @@@ @@@ @@@@)
```

Example of a corresponding LCONF-Section

```text
# Pattern (+@ @@@ @@@ @@@@)
telephone_number :: +1 212 748 4496
```

#### 7.3.1.3. LCONF-Constrain-Min-Max

**TYPE_STRING, TYPE_DIGITS** one MAY set a minimum/maximum LCONF-Constrain-Min-Max which MUST:

* start with a LCONF_LEFT_PARENTHESIS
* followed by a *LCONF-Constrain-Min*: TYPE_INTEGER greater than zero (0)
* followed by a LCONF_COMMA
* followed by a *LCONF-Constrain-Max*: TYPE_INTEGER greater than zero (0)
* followed by a LCONF_RIGHT_PARENTHESIS

IMPORTANT: To not define one of the two constraints set it to TYPE_NOTSET (LCONF_NOTSET).

**TYPE_STRING:** the minimum is the number of required characters and the maximum allowed characters.

**TYPE_DIGITS:** the minimum is the number of required characters (digits) and the maximum allowed characters (digits).

Example LCONF-Schema *ITEM* Pair

```text
. State | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_STRING (2,2)

. credit_card_number | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_DIGITS (15,19)
```

Example of a corresponding LCONF-Section

```text
# minimum required chars 2, maximum allowed chars 2
State :: NY

# minimum required chars (digits) 15, maximum allowed chars (digits) 19
credit_card_number :: 4639588964759877
```

For **TYPE_INTEGER, TYPE_FLOAT and TYPE_NUMBER**  one MAY set a minimum/maximum LCONF-Constrain-Min-Max which MUST:

* start with a LCONF_LEFT_PARENTHESIS
* followed by a *LCONF-Constrain-Min*: Optional LCONF_GREATER_THAN_SIGN followed by TYPE_INTEGER
* followed by a LCONF_COMMA
* followed by a *LCONF-Constrain-Max*: Optional LCONF_LESS_THAN_SIGN followed by TYPE_INTEGER
* followed by a LCONF_RIGHT_PARENTHESIS

IMPORTANT: To not define one of the two constraints set it to TYPE_NOTSET (LCONF_NOTSET).

* **LCONF-Constrain-Min:** the minimum required value

    * if an optional LCONF_GREATER_THAN_SIGN was defined the minimum reguired value greater than

* **LCONF-Constrain-Max:** the maximum allowed value

    * if an optional LCONF_LESS_THAN_SIGN was defined the maximum allowed value less than

Example LCONF-Schema *ITEM* Pair

```text
. ZIPCode | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_INTEGER (10000,99999)

. interest_rate | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_FLOAT (0.5,10.0)

. interest_rate2 | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_NUMBER (>1,<10.0)

. moving_balance | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_FLOAT (>-30000,NOTSET)
```

Example of a corresponding LCONF-Section

```text
# minimum required value 10000, maximum allowed value 99999
ZIPCode :: 10022

# minimum required value 0.5, maximum allowed value 10.0
interest_rate :: 10.0

# minimum required value greater than 1, maximum allowed value less than 10.0
interest_rate2 :: 9.9999

# minimum required value greater than -30000, maximum allowed value not defined
moving_balance :: -29999.9999
```

## 7.4. Schema: *DEFAULT* & *EMPTY_REPLACEMENT* Pairs

DEFAULT and EMPTY_REPLACEMENT are optional items depending on the LCONF-STRUCTURE.

### 7.4.1. LCONF-Schema: *STRUCTURE_PAIR*

DEFAULT and EMPTY_REPLACEMENT are optional items.

If not set it is for both assumed:

* for TYPE_STRING, TYPE_DIGITS, TYPE_PATTERN_DIGITS it is assumed a LCONF_EMPTY_STRING: <br />
    `DEFAULT ::` and `EMPTY_REPLACEMENT ::`
* for all other TYPES it is assumed to be LCONF_NOTSET: <br />
    `DEFAULT :: NOTSET` and `EMPTY_REPLACEMENT :: NOTSET`

Examples:

```text
. registered1 | STRUCTURE_PAIR
    ITEM :: REQUIRED | TYPE_STRING
    DEFAULT ::
    EMPTY_REPLACEMENT ::

. registered2 | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_BOOLEAN
    DEFAULT :: NOTSET
    EMPTY_REPLACEMENT :: NOTSET

# Set DEFAULT and EMPTY_REPLACEMENT
. registered3 | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_BOOLEAN
    DEFAULT :: true
    EMPTY_REPLACEMENT :: false
```

### 7.4.2. LCONF-Schema: *STRUCTURE_LIST*

DEFAULT is an optional item and EMPTY_REPLACEMENT is not a STRUCTURE_LIST LCONF-Schema item.

If not set it is assumed an empty list (without any values - an empty sequence) LCONF_EMPTY_LIST: `- DEFAULT`

Examples:

```text
. colors | STRUCTURE_LIST
    ITEM :: REQUIRED | TYPE_STRING
    - DEFAULT

# Set DEFAULT
. registered1 | STRUCTURE_LIST
    ITEM :: OPTIONAL | TYPE_STRING
    - DEFAULT
        Red,
        blue

# Set DEFAULT: Compact notation
. registered2 | STRUCTURE_LIST
    ITEM :: OPTIONAL | TYPE_STRING
    - DEFAULT :: Red, blue
```

### 7.4.3. LCONF-Schema: *STRUCTURE_TABLE*

DEFAULT is an optional item and EMPTY_REPLACEMENT is not a STRUCTURE_TABLE LCONF-Schema item.

If not set it is assumed a LCONF_EMPTY_TABLE (without any rows - an empty sequence): `| DEFAULT`

**COLUMNS**: This is required and only used for STRUCTURE_TABLE and is a STRUCTURE_LIST where each value line has the
format of:

`Column NAME | Column DEFAULT | Column EMPTY_REPLACEMENT | Column Value-REQUIRED | Column TYPE & any CONSTRAINTS`

Leading and ending whitespace of individual parts is stripped.

Examples:

```text
. favorite1s | STRUCTURE_TABLE
    ITEM :: OPTIONAL
    - COLUMNS
        # NAME   | DEFAULT | EMPTY_REPLACEMENT | Value-REQUIRED     | TYPE & any CONSTRAINTS
        # ------ | ------- | ----------------- | ------------------ | -----------------------
        food     |         |                   | REQUIRED_NOT_EMPTY | TYPE_STRING (NOTSET,25)
        sport    |         |                   | OPTIONAL           | TYPE_STRING
        color    |         |                   | OPTIONAL           | TYPE_STRING
        number   | NOTSET  | 0                 | OPTIONAL           | TYPE_INTEGER (>0,<100)

    | DEFAULT

. favorites2 | STRUCTURE_TABLE
    ITEM :: OPTIONAL
    - COLUMNS
        # NAME   | DEFAULT | EMPTY_REPLACEMENT | Value-REQUIRED     | TYPE & any CONSTRAINTS
        # ------ | ------- | ----------------- | ------------------ | -----------------------
        food     |         |                   | REQUIRED_NOT_EMPTY | TYPE_STRING (NOTSET,25)
        sport    |         |                   | OPTIONAL           | TYPE_STRING
        color    |         |                   | OPTIONAL           | TYPE_STRING
        number   | NOTSET  | 0                 | OPTIONAL           | TYPE_INTEGER (>0,<100)

    | DEFAULT
        # food          | sport          | color  | number |
        # ------------- | -------------- | ------ | ------ |
        | Stroganoff    | figure skating | violet | 0.0    |
        | Rice          | ballet         | orange | NOTSET |
        | French fries  |                |        |        |
        | Fried Chicken | NOTSET         |        | 89.9   |
```

## 7.5 LCONF-Schema-Comment-Lines

If a LCONF-Schema-Line first none whitespace character is a LCONF_SCHEMA_COMMENT_LINE_IDENTIFIER it is considered a
LCONF-Schema-Comment-Line which are parsed.
The purpose of such LCONF-Schema-Comment-Lines is to emit an example LCONF-Section with additional information.

**NOTE**: LCONF-Schema MAY also contain regular LCONF-Section-Comment-Line which are not parsed.

### 7.5.1 Auto-generated Comment-Lines

LCONF-Libraries MUST implement an option to auto-generate for each LCONF-Schema-Definition Comment-Lines which MUST
contain info like:

* LCONF-Key-Name which it referes too and *optional the LCONF-Structure*
* LCONF-Item-Requirement-Option
* LCONF-Value-Type with any constraints
* Default-Value
* Empty-Default-Value

**NOTE** STRUCTURE_TABLE needs to aut-generate also the COLUMNS info.

Example LCONF-Schema-Definition

```text
. registered | STRUCTURE_PAIR
    ITEM :: OPTIONAL | TYPE_BOOLEAN
    DEFAULT :: true
    EMPTY_REPLACEMENT :: false

. interest_rate | STRUCTURE_PAIR
    ITEM :: REQUIRED | TYPE_FLOAT (0.5,10.0)
```

Example default output: **The output format is not defined and are here just as an example.**

```text
# <registered> OPTIONAL | TYPE_BOOLEAN
#   Default: <true> | Empty-Default: <false>
registered :: true

# <interest_rate> REQUIRED | TYPE_FLOAT (0.5,10.0)
#   Default: <NOTSET> | Empty-Default: <NOTSET>
interest_rate :: NOTSET
```

#### 7.5.1.1 STRUCTURE_TABLE

For STRUCTURE_TABLE the auto-generated LCONF-Schema-Comment-Lines MUST also include the Columns definitions.

Example LCONF-Schema-Definition

```text
. favorites | STRUCTURE_TABLE
    ITEM :: OPTIONAL
    - COLUMNS
        # NAME   | DEFAULT | EMPTY_REPLACEMENT | Value-REQUIRED     | TYPE & any CONSTRAINTS
        # ------ | ------- | ----------------- | ------------------ | -----------------------
        food     |         |                   | REQUIRED_NOT_EMPTY | TYPE_STRING (NOTSET,25)
        sport    |         |                   | OPTIONAL           | TYPE_STRING
        color    |         |                   | OPTIONAL           | TYPE_STRING
        number   | NOTSET  | 0                 | OPTIONAL           | TYPE_INTEGER (>0,<100)

    | DEFAULT
        # food          | sport          | color  | number |
        # ------------- | -------------- | ------ | ------ |
        | Stroganoff    | figure skating | violet | 0.0    |
        | Rice          | ballet         | orange | NOTSET |
        | French fries  |                |        |        |
        | Fried Chicken | NOTSET         |        | 89.9   |
```

Example default output: **The output format is not defined and are here just as an example.**

```text
# <favorites> OPTIONAL
# COLUMNS # NAME   | DEFAULT | EMPTY_REPLACEMENT | Value-REQUIRED     | TYPE & any CONSTRAINTS
#         # ------ | ------- | ----------------- | ------------------ | -----------------------
#         food     |         |                   | REQUIRED_NOT_EMPTY | TYPE_STRING (NOTSET,25)
#         sport    |         |                   | OPTIONAL           | TYPE_STRING
#         color    |         |                   | OPTIONAL           | TYPE_STRING
#         number   | NOTSET  | 0                 | OPTIONAL           | TYPE_INTEGER (>0,<100)
| DEFAULT
    # food          | sport          | color  | number |
    # ------------- | -------------- | ------ | ------ |
    | Stroganoff    | figure skating | violet | 0.0    |
    | Rice          | ballet         | orange | NOTSET |
    | French fries  |                |        |        |
    | Fried Chicken | NOTSET         |        | 89.9   |
```

### 7.5.2 LCONF-Section Comment Emit Options

| **Name**                  | **Definition**                                                        |
|:--------------------------|:----------------------------------------------------------------------|
| EMIT_NO_COMMENTS          | No comments MUST be emitted.                                          |
| EMIT_ONLY_MANUAL_COMMENTS | Only manual LCONF-Schema-Comment-Lines MUST be emitted.               |
| EMIT_ALL_COMMENTS         | Auto-generated and manual LCONF-Schema-Comment-Lines MUST be emitted. |

## 7.6. LCONF-Libraries Schema Requirements

* All LCONF-Libraries MUST support the *LCONF_FORMAT_SCHEMA_STRICT* and MAY implement individual
    *LCONF_FORMAT_SCHEMA_FLEXIBLE*.

* LCONF-Libraries SHOULD implement an option to generate optimized copy/past source code from a parsed LCONF_SCHEMA and
    to emit it which removes the need of parsing each time the LCONF_SCHEMA.

* LCONF-Libraries MUST implement an option to emit a parsed LCONF_SCHEMA inclusive any LCONF-Schema-Comment-Lines.

* LCONF-Libraries MUST implement an option to emit a LCONF-Section inclusive any LCONF-Schema-Comment-Lines.

* It is RECOMMENDE that LCONF-Libraries implement an option to generate a generic basic LCONF-Schema from any parsed
    LCONF-Section which could be used as starting point for a corresponding final LCONF-Schema.
