<div align="center">
    <a href="http://lconf-data-serialization-format.github.io/">
        <img src="../../images/lconf-logo.png" alt="LCONF LOGO" title="The LCONF-Data-Serialization-Format Organization">
    </a>
</div>

> This file is part of the *LCONF-Data-Serialization-Format-Standard Documentation*.
>
> Copyright (c) 2014 - 2015, **peter1000**  <https://github.com/peter1000>.

# 3. LCONF Text

A sequence of Unicode characters that MUST contain one or more named LCONF-Sections.

**WARNING:** LCONF-Text MUST NOT contain any LCONF_SECTION_START or LCONF_SECTION_END token in any form except for the
defined purpose. Each LCONF_SECTION_START token MUST be closed by a LCONF_SECTION_END token.

The set of tokens includes structural tokens, literal name tokens as well as diverse LCONF-Value-Types.

## 3.1. Structural Tokens

The structural tokens:

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

## 3.1.1 Optional Structural Tokens

LCONF-Schema-Definitions uses optional constrain structural tokens:

| **Name**                          | **Definition**                                           | **Example**       |
|:----------------------------------|:---------------------------------------------------------|:------------------|
| LCONF-Constrain-Digits-Pattern    | LCONF_LEFT_PARENTHESIS  and LCONF_RIGHT_PARENTHESIS      | (+@ @@@ @@@ @@@@) |
| LCONF-Constrain-Predefined-Values | LCONF_LEFT_SQUARE_BRACKET and LCONF_RIGHT_SQUARE_BRACKET | [value1, Value2]  |
| LCONF-Constrain-Min-Max           | LCONF_LEFT_PARENTHESIS  and LCONF_RIGHT_PARENTHESIS      | (1,12)            |

## 3.2. Literal Name Tokens

The literal name tokens:

| **Name**                     | **Definition**                                                                                    | **Example**  |
|:-----------------------------|:--------------------------------------------------------------------------------------------------|:-------------|
| LCONF_SECTION_START          | U+005F U+005F U+005F U+0053 U+0045 U+0043 U+0054 U+0049 U+004F U+004E                             | `___SECTION` |
| LCONF_SECTION_END            | U+005F U+005F U+005F U+0045 U+004E U+0044                                                         | `___END`     |
| LCONF_SECTION_FORMAT         | U+004C U+0043 U+004F U+004E U+0046                                                                | `LCONF`      |
| LCONF_SCHEMA_FLEXIBLE_FORMAT | U+004C U+0043 U+004F U+004E U+0046 U+0053 U+0044                                                  | `STRICT`     |
| LCONF_SCHEMA_STRICT_FORMAT   | U+004C U+0043 U+004F U+004E U+0046 U+0053 U+0044 U+005F U+0053 U+0054 U+0052 U+0049 U+0043 U+0054 | `FLEXIBLE`   |
| LCONF_TRUE                   | U+0074 U+0072 U+0075 U+0065                                                                       | `true`       |
| LCONF_FALSE                  | U+0066 U+0061 U+006C U+0073 U+0065                                                                | `false`      |
| LCONF_NOTSET                 | U+004E U+004F U+0054 U+0053 U+0045 U+0054                                                         | `NOTSET`     |
| LCONF_FORCE                  | U+0046 U+004F U+0052 U+0043 U+0045                                                                | `FORCE`      |
