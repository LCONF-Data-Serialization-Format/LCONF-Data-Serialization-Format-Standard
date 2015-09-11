<div align="center">
    <a href="http://lconf-data-serialization-format.github.io/">
        <img src="../../images/lconf-logo.png" alt="LCONF LOGO" title="The LCONF-Data-Serialization-Format Organization">
    </a>
</div>

> This file is part of the *LCONF-Data-Serialization-Format-Standard Documentation*.
>
> Copyright (c) 2014 - 2015, **peter1000**  <https://github.com/peter1000>.

# 2. LCONF Terminology

A collection of most LCONF terms and base definitions used in the
*LCONF-Data-Serialization-Format-Standard Documentation*.

## 2.1. Characters

### 2.1.1. Single Characters

| **Name**                   | **Unicode** | **Unicode Name**     | **ASCII Dec** |
|:---------------------------|:------------|:---------------------|:--------------|
| LCONF_SPACE                | U+0020      | SPACE                | 32            |
| LCONF_NUMBER_SIGN          | U+0023      | NUMBER SIGN          | 35            |
| LCONF_LEFT_PARENTHESIS     | U+0028      | LEFT PARENTHESIS     | 40            |
| LCONF_RIGHT_PARENTHESIS    | U+0029      | RIGHT PARENTHESIS    | 41            |
| LCONF_ASTERISK             | U+002A      | ASTERISK             | 42            |
| LCONF_PLUS                 | U+002B      | PLUS SIGN            | 43            |
| LCONF_COMMA                | U+002C      | COMMA                | 44            |
| LCONF_MINUS                | U+002D      | HYPHEN-MINUS         | 45            |
| LCONF_PERIOD               | U+002E      | FULL STOP            | 46            |
| LCONF_SLASH                | U+002F      | SOLIDUS              | 47            |
| LCONF_COLON                | U+003A      | COLON                | 58            |
| LCONF_LESS_THAN_SIGN       | U+003C      | LESS-THAN SIGN       | 60            |
| LCONF_EQUALS_SIGN          | U+003D      | EQUALS SIGN          | 61            |
| LCONF_GREATER_THAN_SIGN    | U+003E      | GREATER-THAN SIGN    | 62            |
| LCONF_AT_SIGN              | U+0040      | COMMERCIAL AT        | 64            |
| LCONF_LEFT_SQUARE_BRACKET  | U+005B      | LEFT SQUARE BRACKET  | 91            |
| LCONF_RIGHT_SQUARE_BRACKET | U+005D      | RIGHT SQUARE BRACKET | 93            |
| LCONF_UNDERSCORE           | U+005F      | LOW LINE             | 95            |
| LCONF_VERTICAL_LINE        | U+007C      | VERTICAL LINE        | 124           |

### 2.1.2. Character Group

| **Name**              | **Unicode**                           | **Unicode Name**                                      | **ASCII Dec**  |
|:----------------------|:--------------------------------------|:------------------------------------------------------|:---------------|
| LCONF_DIGITS          | The code points U+0030 through U+0039 | DIGIT ZERO through DIGIT NINE                         | 48 through 57  |
| LCONF_CAPITAL_LETTERS | The code points U+0041 through U+005A | LATIN CAPITAL LETTER A through LATIN CAPITAL LETTER Z | 65 through 90  |
| LCONF_SMALL_LETTERS   | The code points U+0061 through U+007A | LATIN SMALL LETTER A through LATIN SMALL LETTER Z     | 97 through 122 |

## 2.2. Literal Name Tokens

| **Name**                     | **Definition**                                                        | **Example**  |
|:-----------------------------|:----------------------------------------------------------------------|:-------------|
| LCONF_SECTION_START          | U+005F U+005F U+005F U+0053 U+0045 U+0043 U+0054 U+0049 U+004F U+004E | `___SECTION` |
| LCONF_SECTION_END            | U+005F U+005F U+005F U+0045 U+004E U+0044                             | `___END`     |
| LCONF_FORMAT_LCONF           | U+004C U+0043 U+004F U+004E U+0046                                    | `LCONF`      |
| LCONF_FORMAT_SCHEMA_STRICT   | U+0053 U+0054 U+0052 U+0049 U+0043 U+0054                             | `STRICT`     |
| LCONF_FORMAT_SCHEMA_FLEXIBLE | U+0046 U+004C U+0045 U+0058 U+0049 U+0042 U+004C U+0045               | `FLEXIBLE`   |
| LCONF_TRUE                   | U+0074 U+0072 U+0075 U+0065                                           | `true`       |
| LCONF_FALSE                  | U+0066 U+0061 U+006C U+0073 U+0065                                    | `false`      |
| LCONF_NOTSET                 | U+004E U+004F U+0054 U+0053 U+0045 U+0054                             | `NOTSET`     |
| LCONF_FORCE                  | U+0046 U+004F U+0052 U+0043 U+0045                                    | `FORCE`      |

* `___SECTION` is a reserved LCONF character sequence.
* `___END`     is a reserved LCONF character sequence.
* `NOTSET`     is a reserved LCONF character sequence.

## 2.3. LCONF Structures

| **Name**                 | **Definition**                                                              |
|:-------------------------|:----------------------------------------------------------------------------|
| STRUCTURE_PAIR           | Associates a LCONF-Key-Name with one data value.                            |
| STRUCTURE_TABLE          | Associates a LCONF-Key-Name with ordered tabular-data (columns and rows).   |
| STRUCTURE_LIST           | Associates a LCONF-Key-Name with an ordered sequence (list) of data values. |
| STRUCTURE_SINGLE_BLOCK   | A collection of any of the six LCONF-Structures.                            |
| STRUCTURE_NAMED_BLOCKS   | A collection of repeated named STRUCTURE_SINGLE_BLOCKs.                     |
| STRUCTURE_UNNAMED_BLOCKS | A collection of repeated unnamed STRUCTURE_SINGLE_BLOCKs.                   |

## 2.4. Structural Tokens

| **Name**                             | **Definition**           | **Example** |
|:-------------------------------------|:-------------------------|:------------|
| STRUCTURE_LIST_IDENTIFIER            | LCONF_MINUS              |             |
| STRUCTURE_LIST_VALUE_SEPARATOR       | LCONF_COMMA              |             |
| STRUCTURE_TABLE_IDENTIFIER           | LCONF_VERTICAL_LINE      |             |
| STRUCTURE_TABLE_VALUE_SEPARATOR      | LCONF_VERTICAL_LINE      |             |
| STRUCTURE_SINGLE_BLOCK_IDENTIFIER    | LCONF_PERIOD             |             |
| STRUCTURE_NAMED_BLOCKS_IDENTIFIER    | LCONF_ASTERISK           |             |
| STRUCTURE_UNNAMED_BLOCKS_IDENTIFIER  | LCONF_ASTERISK           |             |
| LCONF_SINGLE_BLOCK_REUSE             | Double LCONF_EQUALS_SIGN | `==`        |
| LCONF_SCHEMA_SEPARATOR               | LCONF_VERTICAL_LINE      |             |
| LCONF_SCHEMA_COMMENT_LINE_IDENTIFIER | LCONF_SLASH              |             |
| LCONF_COMMENT_LINE_IDENTIFIER        | LCONF_NUMBER_SIGN        |             |
| LCONF_KEY_VALUE_SEPARATOR            | Double LCONF_COLON       | `::`        |

* STRUCTURE_BLOCKS_IDENTIFIER: LCONF_ASTERISK common indentifier for STRUCTURE_NAMED_BLOCKS_IDENTIFIER and
    STRUCTURE_UNNAMED_BLOCKS_IDENTIFIER

### 2.4.1. Optional Structural Tokens

LCONF-Schema-Definitions uses optional constrain structural tokens:

| **Name**                          | **Definition**                                           | **Example**       |
|:----------------------------------|:---------------------------------------------------------|:------------------|
| LCONF-Constrain-Digits-Pattern    | LCONF_LEFT_PARENTHESIS  and LCONF_RIGHT_PARENTHESIS      | (+@ @@@ @@@ @@@@) |
| LCONF-Constrain-Predefined-Values | LCONF_LEFT_SQUARE_BRACKET and LCONF_RIGHT_SQUARE_BRACKET | [value1, Value2]  |
| LCONF-Constrain-Min-Max           | LCONF_LEFT_PARENTHESIS  and LCONF_RIGHT_PARENTHESIS      | (1,12)            |

## 2.5. LCONF Key-Names

| **Name**          | **Definition**                                                                                                                 |
|:------------------|:-------------------------------------------------------------------------------------------------------------------------------|
| LCONF-Key-Name    | A sequence of one or more Unicode characters and is case-sensitive. A LCONF-Key-Name MUST NOT contain any LCONF_VERTICAL_LINE. |
| LCONF-Column-Name | STRUCTURE_TABLE's Column-Names are also considered to be LCONF-Key-Names.                                                      |

**NOTE:** LCONF-Schema-Definitions MAY add additional constraints to LCONF-Key-Names.

## 2.6. LCONF Value Types

### 2.6.1. NOTSET

| **Name**    | **Definition**                                  |
|:------------|:------------------------------------------------|
| TYPE_NOTSET | The literal name token LCONF_NOTSET (`NOTSET`). |

TYPE_NOTSET: is used to indicate the lack of a value and is different from an Empty-Value.

### 2.6.2. String

| **Name**            | **Definition**                                                                           |
|:--------------------|:-----------------------------------------------------------------------------------------|
| TYPE_STRING         | A sequence of zero or more Unicode characters.                                           |
| TYPE_DIGITS         | A TYPE_STRING constraint to only LCONF_DIGITS.                                           |
| TYPE_PATTERN_DIGITS | A TYPE_STRING constraint to a pattern where only the individual LCONF_DIGITS MAY change. |

* **TYPE_PATTERN_DIGITS** format uses the LCONF_AT_SIGN `@` as placeholder for expected digits.

    `@@@-@@-@@@@`  could be used as pattern for `123-55-6678`

### 2.6.3. Booleans

| **Name**     | **Definition**                                                     |
|:-------------|:-------------------------------------------------------------------|
| TYPE_BOOLEAN | LCONF_TRUE or LCONF_FALSE.  Literal Name Token: `true` or `false`. |

### 2.6.4. Numbers

| **Name**                     | **Definition**                                                                                                     |
|:-----------------------------|:-------------------------------------------------------------------------------------------------------------------|
| LCONF-Number-Fractional-Part | A LCONF_PERIOD followed by one or more LCONF_DIGITS.                                                               |
| LCONF-Number-E_Exponent      | The code point U+0065 (LATIN SMALL LETTER E) or U+0045 (LATIN CAPITAL LETTER E)                                    |
| LCONF-Number-Exponent-Part   | A LCONF-Number-E_Exponent followed by one or more LCONF_DIGITS optionally prefixed by a LCONF_PLUS or LCONF_MINUS. |

| **Name**     | **Definition**                                                                     |
|:-------------|:-----------------------------------------------------------------------------------|
| TYPE_INTEGER | MUST contain only LCONF_DIGITS. It MAY have a preceding LCONF_PLUS or LCONF_MINUS. |
| TYPE_FLOAT   | supports four different notations.                                                 |
| TYPE_NUMBER  | can be any of TYPE_INTEGER or TYPE_FLOAT.                                          |

* TYPE_INTEGER: 64 bit (signed long) range expected (`-9223372036854775808` to `+9223372036854775807`).

    * LCONF_INTEGER_LOWEST  = -9223372036854775808
    * LCONF_INTEGER_HIGHEST = +9223372036854775807

* TYPE_FLOAT: supports four different notations.

    * Fractional:                    `+3.1415`,  `-3.1415`
    * Exponent:                      `5e+22`,    `-2E-2`
    * Fractional And Exponent Mixed: `6.196E63`, `-1.54e-003`
    * Fraction P/Q Of Two Integers:  `+3/4`,     `-93/16`,    `1/8`,   `2789/-598`s

### 2.6.5.  Date / Time Separators

| **Name**                  | **Definition**                                                       |
|:--------------------------|:---------------------------------------------------------------------|
| LCONF-Date-Separator      | A LCONF_MINUS is used as separator for date parts.                   |
| LCONF-Time-Separator      | A LCONF_COLON is used as separator for time parts.                   |
| LCONF-Date-Time-Separator | A code point U+0054 (LATIN CAPITAL LETTER T) `" T "` or LCONF_SPACE. |

### 2.6.6.  Dates

| **Name**    | **Definition**                                                |
|:------------|:--------------------------------------------------------------|
| LCONF-Year  | [YYYY] indicates a four-digit year, 0000 through 9999.        |
| LCONF-Month | [MM] indicates a two-digit month of that year, 01 through 12. |
| LCONF-Day   | [DD] indicates a two-digit day of that month, 01 through 31.  |

| **Name**   | **Definition**                 |
|:-----------|:-------------------------------|
| TYPE_MONTH | `YYYY-MM` e.g.: `1945-03`      |
| TYPE_DAY   | `YYYY-MM-DD` e.g. `2014-11-15` |

### 2.6.7. Times

| **Name**                   | **Definition**                                                                                                 |
|:---------------------------|:---------------------------------------------------------------------------------------------------------------|
| LCONF-Hour                 | [hh] refers to a zero-padded hour between 00 and 24 (24 only to denote midnight at the end of a day)           |
| LCONF-Minute               | [mm] refers to a zero-padded minute between 00 and 59.                                                         |
| LCONF-Second               | [ss] refers to a zero-padded second between 00 and 60 (60 only to denote an added leap second).                |
| LCONF-Second-Fraction-Part | [fff] refers to LCONF_PERIOD followed by one or more LCONF_DIGITS representing a decimal fraction of a second. |

| **Name**             | **Definition**                                                      |
|:---------------------|:--------------------------------------------------------------------|
| TYPE_MINUTE          | `hh:mm` e.g: `12:30`                                                |
| TYPE_SECOND          | `hh:mm:ss` e.g: `02:30:42`                                          |
| TYPE_SECOND_FRACTION | `hh:mm:ss.fff` e.g: `12:30:59.001`, `04:02:00.000156`, `18:53:16.1` |

### 2.6.8. Date/Times

| **Name**                  | **Definition**                                              |
|:--------------------------|:------------------------------------------------------------|
| TYPE_DAY_MINUTE1          | `YYYY-MM-DD hh:mm` e.g: `2013-07-01 12:30`                  |
| TYPE_DAY_MINUTE2          | `YYYY-MM-DDThh:mm` e.g: `2013-07-01T12:30`                  |
|                           |                                                             |
| TYPE_DAY_SECOND1          | `YYYY-MM-DD hh:mm:ss` e.g: `2013-07-01 12:30:59`            |
| TYPE_DAY_SECOND2          | `YYYY-MM-DDThh:mm:ss` e.g: `2013-07-01T12:30:59`            |
|                           |                                                             |
| TYPE_DAY_SECOND_FRACTION1 | `YYYY-MM-DD hh:mm:ss.fff` e.g: `2013-07-01 04:02:00.000156` |
| TYPE_DAY_SECOND_FRACTION2 | `YYYY-MM-DDThh:mm:ss.fff` e.g: `2013-07-01T12:30:59.001`    |

### 2.6.8. Ranges

| **Name**                       | **Definition**                                                                   |
|:-------------------------------|:---------------------------------------------------------------------------------|
| LCONF-Range-Part-Separator     | A LCONF_VERTICAL_LINE is used as separator for range parts.                      |
| LCONF-Range-Start-Number       | A LCONF-Number                                                                   |
| LCONF-Range-Step-Number        | Any LCONF-Number MUST NOT be equal to 0 (zero)                                   |
| LCONF-Range-End-Number         | A LCONF-Number                                                                   |
| LCONF-Range-Number-Of-Elements | LCONF_ASTERISK followed by a LCONF-Integer greater than zero without LCONF_PLUS. |

* TYPE_RANGE_OF_ELEMENTS:

    * LCONF-Range-Start-Number|LCONF-Range-Step-Number|LCONF-Range-Number-Of-Elements <br />
        Example: `-10|1|*21`, `512.4|0.125|*8`

* TYPE_RANGE_BY_END_VALUE:

    * LCONF-Range-Start-Number|LCONF-Range-Step-Number|LCONF-Range-End-Number <br />
        `100.8|1.27|106`
    * LCONF-Range-Start-Number|LCONF-Range-Step-Number|LCONF-Range-End-Number|FORCE <br />
        `100.8|1.27|106|FORCE`

### 2.7. LCONF-Item-Requirement-Option

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

### 2.8 LCONF-Section Comment Emit Options

| **Name**                  | **Definition**                                                        |
|:--------------------------|:----------------------------------------------------------------------|
| EMIT_NO_COMMENTS          | No comments MUST be emitted.                                          |
| EMIT_ONLY_MANUAL_COMMENTS | Only manual LCONF-Schema-Comment-Lines MUST be emitted.               |
| EMIT_ALL_COMMENTS         | Auto-generated and manual LCONF-Schema-Comment-Lines MUST be emitted. |

### 2.9. Diverse Other Terms

| **Name**                    | **Definition**                                                                                                                                                                       |
|:----------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| LCONF-Text                  | A sequence of Unicode characters that MUST contain one or more LCONF-Sections.                                                                                                       |
| LCONF-Section               | A sequence of Unicode characters starting with a LCONF_SECTION_START token and ending with the first found LCONF_SECTION_END token.                                                  |
| LCONF-Schema                | A sequence of Unicode characters starting with a LCONF_SECTION_START token and ending with the first found LCONF_SECTION_END token.                                                  |
| LCONF-Section-Name          | A sequence of one or more Unicode characters.                                                                                                                                        |
| LCONF-Indentation-Per-Level | One LCONF-Digit (DIGIT TWO THROUGH DIGIT EIGHT) minimum 2 and maximum 8.                                                                                                             |
| LCONF_BLANK_LINE            | A line that contains only whitespace characters (zero or more).                                                                                                                      |
| LCONF_NONE_BLANK_LINE       | A line that contains one or more none whitespace characters.                                                                                                                         |
| LCONF_EMPTY_STRING          | A sequence of zero Unicode characters.                                                                                                                                               |
| LCONF_EMPTY_LIST            | An empty sequence.                                                                                                                                                                   |
| LCONF_EMPTY_TABLE           | An empty sequence of tabular-data (columns and rows): no table rows.                                                                                                                 |
| LCONF_EMPTY_REPEATED_BLOCK  | STRUCTURE_NAMED_BLOCKS or STRUCTURE_UNNAMED_BLOCKS: An empty collection.                                                                                                             |
| LCONF_EMPTY_SINGLE_BLOCK    | STRUCTURE_SINGLE_BLOCK: An empty collection.                                                                                                                                         |
| LCONF-File-Extension        | The official extension for LCONF-Data-Serialization-Format files is `.lconf` (U+002E U+006C U+0063 U+006F U+006E U+0066)                                                             |
| LCONFSD-File-Extension      | The official extension for LCONF-Data-Serialization-Format Schema-Definition files is `.lconfsd` (U+002E U+006C U+0063 U+006F U+006E U+0066 U+0073 U+0064)                           |
| LCONF-Template-Structure    | A specific implementation of a LCONF-Section related code portion with all default values and value types based on corresponding LCONF-Data-Serialization-Format Schema-Definitions. |
