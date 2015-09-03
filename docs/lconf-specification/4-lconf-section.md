<div align="center">
    <a href="http://lconf-data-serialization-format.github.io/">
        <img src="../../images/lconf-logo.png" alt="LCONF LOGO" title="The LCONF-Data-Serialization-Format Organization">
    </a>
</div>

> This file is part of the *LCONF-Data-Serialization-Format-Standard Documentation*.
>
> Copyright (c) 2014 - 2015, **peter1000**  <https://github.com/peter1000>.

# 4. LCONF Section

A sequence of Unicode characters starting with a LCONF_SECTION_START token and ending with the first found
LCONF_SECTION_END token. A LCONF-Section MUST NOT contain any additional LCONF_SECTION_START
tokens.

* Any text before or after a valid LCONF-Section is considered additional text.
* The LCONF_SECTION_START and  LCONF_SECTION_END token lines MUST NOT contain any indentation and MUST NOT be on the
    same line.

Below is an example of a LCONF-Text containing two LCONF-Sections and additional text.

```text
Any text before or after a valid LCONF-Section is considered additional text.

___SECTION :: 4 :: LCONF :: Example1
Color :: Blue
FONT :: Liberation Mono
___END

Some more intermitten text followed by a second LCONF-Section.

___SECTION :: 4 :: LCONF :: Example2
Color :: Red
FONT :: Open Sans
___END
```

A valid but Empty LCONF-Section.

```text
___SECTION :: 2 :: LCONF :: Example
___END
```

A valid but *not recommendet* LCONF-Section. It is valid because any Text befor the LCONF_SECTION_START token is
disregarded - so the actual data starts without any indentation and is the same as the previous Example.

```text
Any text before or after a valid LCONF-Section is considered additional text.___SECTION :: 2 :: LCONF :: Example
___END
```

Every LCONF-Section has a LCONF-Section-Name defined in the LCONF-Section-Start-Line.

```text
___SECTION :: 2 :: LCONF :: Example Section Name
___END
```

## 4.1. Section Start Line

```text
___SECTION :: LCONF-Indentation-Per-Level :: LCONF_SECTION_FORMAT or LCONF_SCHEMA_FLEXIBLE_FORMAT or LCONF_SCHEMA_STRICT_FORMAT:: LCONF-Section-Name
```

Each LCONF-Section-Start-Line MUST

* start with the LCONF_SECTION_START token
* followed by one LCONF_SPACE
* followed by one LCONF_KEY_VALUE_SEPARATOR
* followed by one LCONF_SPACE
* followed by a LCONF-Indentation-Per-Level
* followed by one LCONF_SPACE
* followed by one LCONF_KEY_VALUE_SEPARATOR
* followed by one LCONF_SPACE
* followed be a LCONF_SECTION_FORMAT or LCONF_SCHEMA_STRICT_FORMAT or LCONF_SCHEMA_FLEXIBLE_FORMAT
* followed by one LCONF_SPACE
* followed by one LCONF_KEY_VALUE_SEPARATOR
* followed by one LCONF_SPACE
* followed by a LCONF-Section-Name

Example of a LCONF-Section-Start-Line.

```text
___SECTION :: 4 :: LCONF :: Menu Configuration`
```

```text
___SECTION :: 4 :: STRICT :: Menu Configuration Schema Definition`
```

```text
___SECTION :: 4 :: FLEXIBLE :: Menu Configuration Schema Definition`
```

## 4.2. Blank & NONE Blank Line

* LCONF_BLANK_LINE: A line that contains only whitespace characters (zero or more).
* LCONF_NONE_BLANK_LINE: A line that contains one or more none whitespace characters.

## 4.3. Indentation Level

Leading whitespace (LCONF_SPACEs) at the beginning of a LCONF-Section-Line is used to compute the indentation level of
the line. Within a LCONF-Section only LCONF_SPACEs can be used as indentation. The number of LCONF_SPACEs used per
LCONF-Indentation-Level is set as part of the LCONF-Section-Start-Line.

The total number of spaces preceding the first non-blank character then determines the line’s indentation.

The LCONF-Data-Serialization-Format uses LCONF-Indentation for readability and as part of its structure.

## 4.4. Trailing Space

LCONF-Sections MUST NOT contain any lines with *Trailing Space*.

## 4.5. Comment Line

The LCONF-Data-Serialization-Format makes a distinction between two types of LCONF-Comment-Lines:

LCONF-Comment-Lines SHOULD have the indentation level of the following LCONF_NONE_BLANK_LINE.

* LCONF-Section-Comment-Line: If a LCONF-Section-Line first none whitespace character is a
    LCONF_COMMENT_LINE_IDENTIFIER it is considered a LCONF-Section-Comment-Line which are not parsed.
    The usual purpose of a comment line is to communicate between the human maintainers of a file. A typical example is
    comments in a configuration file.

* LCONF-Schema-Comment-Line: If a LCONF-Schema-Line first none whitespace character is a
    LCONF_SCHEMA_COMMENT_LINE_IDENTIFIER it is considered a LCONF-Schema-Comment-Line which are parsed.
    The purpose of such LCONF-Schema-Comment-Lines is to emit an example LCONF-Section with additional information.

    **NOTE**: LCONF-Schema MAY also contain regular LCONF-Section-Comment-Line which are not parsed.

Example of a LCONF-Section-Comment-Line.

```text
___SECTION :: 4 :: LCONF :: Example
# Comment-Line more info: Registry is a STRUCTURE_SINGLE_BLOCK
. Registry
    Name :: Tom Serjo
    - EmailAddresses
        # Comment-Line should have the indentation level of the following LCONF_NONE_BLANK_LINE
        tom1@nomail.net
        tom2@nomail.net

    # Comment-Line should have the indentation level of the following LCONF_NONE_BLANK_LINE

    City :: Paris
___END
```

It is RECOMMENDED that any LCONF-Comment-Line uses at least one LCONF_SPACE after the LCONF_COMMENT_LINE_IDENTIFIER.

```text
___SECTION :: 4 :: LCONF :: Example
#Comment-Line without space between the Identifier and the comment is valid but SHOUD BE AVOIDED.
Name :: Tom Serjo

# EmailAddresses: Usually one should use one space between the Identifier and the comment text.
#                 But sometimes one wants to aline a multi-line comment in a certain style.
- EmailAddresses
    tom1@nomail.net
    tom2@nomail.net
City :: Paris
___END
```

Example of LCONF-Schema-Comment-Lines which are parsed.

```text
___SECTION :: 4 :: STRICT :: lconf_standard_example1
/ <FamilyName>: Please use for the first letter uppercase followed by lowercase letters.
/               e.g. `Williams`
. FamilyName | STRUCTURE_PAIR
    ITEM :: REQUIRED_NOT_EMPTY | TYPE_STRING
    DEFAULT ::
    EMPTY_REPLACEMENT ::
___END
```
