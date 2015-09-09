<div align="center">
    <a href="http://lconf-data-serialization-format.github.io/">
        <img src="../../images/lconf-logo.png" alt="LCONF LOGO" title="The LCONF-Data-Serialization-Format Organization">
    </a>
</div>

> This file is part of the *LCONF-Data-Serialization-Format-Standard Documentation*.
>
> Copyright (c) 2014 - 2015, **peter1000**  <https://github.com/peter1000>.

# 5. LCONF Structures

The set of six structures includes three simple structures and three collection structures.

The three simple structures are:

* STRUCTURE_PAIR
* STRUCTURE_LIST (inclusive Compact_STRUCTURE_LIST notation)
* STRUCTURE_TABLE

The three collection structures are:

* STRUCTURE_SINGLE_BLOCK
* STRUCTURE_NAMED_BLOCKS
* STRUCTURE_UNNAMED_BLOCKS

The **LCONF-Root** (Structure) is a special STRUCTURE_SINGLE_BLOCK which exists in every valid LCONF-Section.
It is special in the sense that there is no STRUCTURE_SINGLE_BLOCK_IDENTIFIER line and that it is an unnamed STRUCTURE_SINGLE_BLOCK.

## 5.1. LCONF-Key-Name

Nearly all LCONF-Key-Names will be implemented in a corresponding LCONF-Schema-Definition.
Only few e.g. STRUCTURE_NAMED_BLOCKS-Item (STRUCTURE_SINGLE_BLOCKs) Key-Names will be defined within a LCONF-Section.

## 5.2. STRUCTURE_PAIR

Associates a LCONF-Key-Name with one data value. <br />
The STRUCTURE_PAIR-Value MUST be a sequence of **zero or more** Unicode characters and includes all till the end of
the line. The STRUCTURE_PAIR-Value MAY be a TYPE_NOTSET. (This can be contraint in the LCONF-Schema) <br />
The first value character MUST NOT be any whitespace.

```text
LCONF-Key-Name LCONF_KEY_VALUE_SEPARATOR Value

FirstName :: Mary
```

### 5.2.1. STRUCTURE_PAIR With Empty-Value

This is an exception and MUST use an **empty sequence** of Unicode characters and to avoid a Trailing Whitespace there
MUST be no additional LCONF_SPACE after the LCONF_KEY_VALUE_SEPARATOR.

STRUCTURE_PAIR with an Empty-Value set MUST

* start with a LCONF-Key-Name
* followed by one LCONF_SPACE
* followed by one LCONF_KEY_VALUE_SEPARATOR

This will overwrite the default value with whatever the implementation for an Empty-Value for this STRUCTURE_PAIR
is.

```text
registered ::
```

### 5.2.2. STRUCTURE_PAIR With NOTSET Value

The Value MUST be a TYPE_NOTSET.

STRUCTURE_PAIR with NOTSET value MUST

* start with a LCONF-Key-Name
* followed by one LCONF_SPACE
* followed by one LCONF_KEY_VALUE_SEPARATOR
* followed by one LCONF_SPACE
* followed by a LCONF_NOTSET

This will keep the default STRUCTURE_PAIR value as implemented in a LCONF-Schema-Definition and is the same as
if the STRUCTURE_PAIR was not included in a LCONF-Section.

```text
registered :: NOTSET
```

### 5.2.3. STRUCTURE_PAIR With Assigned Value

The Value MUST be a sequence of **one or more** Unicode characters and includes all till the end of the line.

STRUCTURE_PAIR with assigned value MUST

* start with a LCONF-Key-Name
* followed by one LCONF_SPACE
* followed by one LCONF_KEY_VALUE_SEPARATOR
* followed by one LCONF_SPACE
* followed by a TYPE_NOTSET

This will overwrite the default value with `true`.

```text
registered :: true
```

## 5.3. STRUCTURE_LIST

Associates a LCONF-Key-Name with an ordered sequence (list) of data values. <br />
The first List-Value character MUST NOT be any whitespace.

The STRUCTURE_LIST Value MUST be a sequence of **one or more** Unicode characters and includes all till the end of
the line. The List-Value MAY be a TYPE_NOTSET. (This can be contraint in the LCONF-Schema) <br />
STRUCTURE_LIST-Values use one additional LCONF-Indentation-Per-Level and each List-Value MUST start on a separate line.

A STRUCTURE_LIST_IDENTIFIER-Line MUST

* start with a STRUCTURE_LIST_IDENTIFIER
* followed by one LCONF_SPACE
* followed by a LCONF-Key-Name

```text
- Color_List
```

Values are separate lines.

```text
- Color_List
    Black
    White
    NOTSET
    Blue
```

### 5.3.1. Default STRUCTURE_LIST

To keep the default STRUCTURE_LIST values as implemented in a LCONF-Schema-Definition the STRUCTURE_LIST
MUST NOT be included in a LCONF-Section.

### 5.3.2. Empty STRUCTURE_LIST

Empty STRUCTURE_LIST MUST only define the STRUCTURE_LIST_IDENTIFIER-Line and MUST NOT set any
STRUCTURE_LIST-Value-Lines.

```text
- color_list
```

This will overwrite the default value with an empty List (with values).

### 5.3.3.  Compact_STRUCTURE_LIST notation

Any STRUCTURE_LIST can also be written in a oneline compact notation:  Compact_STRUCTURE_LIST Value MUST be a sequence
of **one or more** Unicode characters and MAY be a TYPE_NOTSET. Multiple Values are separated by
STRUCTURE_LIST_VALUE_SEPARATOR. The Compact_STRUCTURE_LIST MUST NOT end with a STRUCTURE_LIST_VALUE_SEPARATOR. <br />
Leading or ending whitespace of Compact_STRUCTURE_LIST Values is stripped.

```text
STRUCTURE_LIST_IDENTIFIER Compact_STRUCTURE_LIST-Key-Name LCONF_KEY_VALUE_SEPARATOR Value1, Value2, Value3

- colors :: red, blue, green
```

#### 5.3.3.1. Default Compact_STRUCTURE_LIST

To keep the default Compact_STRUCTURE_LIST values as implemented in a LCONF-Schema-Definition the
Compact_STRUCTURE_LIST MUST NOT be included in a LCONF-Section.

#### 5.3.3.2. Empty Compact_STRUCTURE_LIST

There is NO Empty Compact_STRUCTURE_LISTt notation. Just use a Empty General-List.

```text
- color_list
```

#### 5.3.3.3. Compact-List With Assigned Value

LCONF-Compact-List with assigned value MUST

* start with a STRUCTURE_LIST_IDENTIFIER
* followed by one LCONF_SPACE
* followed by a LCONF-Key-Name
* followed by one LCONF_SPACE
* followed by one LCONF_KEY_VALUE_SEPARATOR
* followed by one or more List-Value (multiple List-Values are separated by STRUCTURE_LIST_VALUE_SEPARATOR)

This will overwrite the default value for the List with `Tim, Tom`.

```text
- registered :: Tim, Tom

- registered :: Tim,Tom

- registered :: Tim     ,     Tom
```

## 5.4. STRUCTURE_TABLE

Associates a LCONF-Key-Name with ordered tabular-data (columns and rows).  <br />
A Column-Value MUST be a sequence of **zero or more** Unicode characters and MAY be a TYPE_NOTSET. (This can be
contraint in the LCONF-Schema) <br />
The STRUCTURE_TABLE_IDENTIFIER-Line MUST NOT end with a STRUCTURE_TABLE_VALUE_SEPARATOR.

LCONF-Column-Names MUST be unique within one STRUCTURE_TABLE.

A STRUCTURE_TABLE_IDENTIFIER-Line MUST

* start with a STRUCTURE_TABLE_IDENTIFIER
* followed by one LCONF_SPACE
* followed by a LCONF-Key-Name

```text
| ColorTable
```

STRUCTURE_TABLE-Rows use one additional LCONF-Indentation-Per-Level and each row MUST start on a separate line.
Column-Values of Table-Rows are embraced and separated by STRUCTURE_TABLE_VALUE_SEPARATORs. A STRUCTURE_TABLE-Row MUST
contain the same number of values as column-names specified in the LCONF-Schema-Definition. <br />
Leading or ending whitespace of STRUCTURE_TABLE Column-Values is stripped.

A STRUCTURE_TABLE-Row-Line MUST

* start with one additional LCONF-Indentation-Per-Level
* followed by one STRUCTURE_TABLE_VALUE_SEPARATOR
* followed by a Column-Value which MAY contain leading and ending spaces which will be stripped.
* followed by one STRUCTURE_TABLE_VALUE_SEPARATOR
* *followed by any additional Column-Value and STRUCTURE_TABLE_VALUE_SEPARATOR*

```text
STRUCTURE_TABLE_IDENTIFIER LCONF-Key-Name
    STRUCTURE_TABLE_VALUE_SEPARATOR Column-Values STRUCTURE_TABLE_VALUE_SEPARATOR Column-Values STRUCTURE_TABLE_VALUE_SEPARATOR
    STRUCTURE_TABLE_VALUE_SEPARATOR Column-Values STRUCTURE_TABLE_VALUE_SEPARATOR Column-Values STRUCTURE_TABLE_VALUE_SEPARATOR

| TableKeyName_example
    | Value Column1 Row1 | Value Column2 Row1 |
    | Value Column1 Row2 | Value Column2 Row2 |
```

### 5.4.1. Default STRUCTURE_TABLE

To keep the default STRUCTURE_TABLE values as implemented in a LCONF-Schema-Definition the STRUCTURE_TABLE MUST NOT be
included in a LCONF-Section.

### 5.4.2. Empty STRUCTURE_TABLE

Empty STRUCTURE_TABLEs MUST only define the STRUCTURE_TABLE_IDENTIFIER-Line and MUST NOT set any
STRUCTURE_TABLE-Row-Lines.

This will overwrite the default value with an empty table (with no rows).

```text
| Colors_RGB
```

### 5.4.3. STRUCTURE_TABLE With Rows

```text
# Comment: below is a STRUCTURE_TABLE with a Comment-Line of Column-Name
| Colors_RGB
    #   Color Name| Red| Green| Blue|
    |  forestgreen|  34|   139|   34|
    |        brick| 156|   102|   31|

# Comment: Alignment is not important
| Colors_RGB2
    |forestgreen|34|139|34|
    |brick|156|102|31|

# COMMENT: all Column-Value are empty (missing) - both Table-Rows will be the same.
| Colors_RGB3
    |||||
    |     |     |     |     |

# Comment: below is STRUCTURE_TABLE with empty Column-Values and TYPE_NOTSET Column-Values
#          It uses also 2 Comment-Lines given additional information about the Column-Names so that the whole
#          STRUCTURE_TABLE looks very similar to an markdown table.
| people_table
    # ID | name  | height_cm | weight_kg | age    | registered |
    #:---|:------|:----------|:----------|:-------|:-----------|
    | 1  | Tim   | 178       | 86        | 37     | true       |
    | 2  | Paula | 156       | NOTSET    | NOTSET |            |
    | 3  |       | 186       | 84        | 23     |            |
    | 4  | Dora  | 173       | NOTSET    | 45     | false      |
```

* **Columns With Empty Column-Values**: e.g. Row:3, Column2: `name` - has an empty Column-Value

    This will overwrite the default value for the *Column2: name* with whatever the implementation for an empty
    Column-Value is (example it could be an empty String). Usually it will depend on the Column LCONF-Value-Type.

    e.g. Row:2, Column6: `registered` - has an empty Column-Value <br />

    This Column might have a TYPE_BOOLEAN Value-Type implemented with the Default-Value set to *false* and an
    Empty-Value implemented as *TYPE_NOTSET*.

* **Columns With TYPE_NOTSET Column-Values**:

    This will use the default value for the *Column2: name* (example it could be: *Anonymous*).

* **Columns With Column-Values**: e.g. Row:1, Column2: `name` - has a set Column-Value (more than one Unicode character)

    This will overwrite the default value for the *Column: name* with `Tim`.

## 5.5. STRUCTURE_SINGLE_BLOCK

A collection of any of the six LCONF-Structures. All STRUCTURE_SINGLE_BLOCK-Items (Key-Names) MUST be unique. A
LCONF-Library MUST implement an option to loop over the collection in order.
STRUCTURE_SINGLE_BLOCK-Items use one additional LCONF-Indentation-Per-Level and each Block-Item MUST start on a
separate line.

* **LCONF-Root**

    The LCONF-Root (Structure) is a special STRUCTURE_SINGLE_BLOCK which exists in every valid LCONF-Section.
    It is special in the sense that there is no STRUCTURE_SINGLE_BLOCK_IDENTIFIER line and that it is an unnamed STRUCTURE_SINGLE_BLOCK.

A STRUCTURE_SINGLE_BLOCK-Identifier-Line MUST

* start with a STRUCTURE_SINGLE_BLOCK_IDENTIFIER
* followed by one LCONF_SPACE
* followed by a LCONF-Key-Name

```text
. Color
```

Block-Items are separate lines in the example below these are: `Name :: Blue, - rgb_list, menu :: false`.

```text
. Color
    Name :: Blue
    - rgb_list
        157
        174
        235
    menu :: false
```

### 5.5.1. Default STRUCTURE_SINGLE_BLOCK

To keep the default STRUCTURE_SINGLE_BLOCK values as implemented in a LCONF-Schema-Definition the
STRUCTURE_SINGLE_BLOCK SHOULD NOT be included in a LCONF-Section. (This also depends if any Block-Items are
*REQUIRED*.)

It is PERMITTED to include an Empty STRUCTURE_SINGLE_BLOCK which will also keep the default STRUCTURE_SINGLE_BLOCK
values as implemented in a LCONF-Schema-Definition.
In some cases this might be useful: e.g. if one wants previous comment lines.

Empty STRUCTURE_SINGLE_BLOCK MUST only define the STRUCTURE_SINGLE_BLOCK-Identifier-Line and MUST NOT set any
LCONF-Block-Item-Lines.

```text
. Color
```

```text
___SECTION :: 4 :: LCONF :: SectionName
. STRUCTURE_SINGLE_BLOCK_Identifier_name
    single_block_item1_key :: single_block_item1_value
    - single_block_item2_key list
        my List-Item 1
        my List-Item 2
    # Comment: Blocks can also have other (nested) Blocks
    . inner_single_block_item3_key
        inner_single_block_item1_key :: inner_single_block_item1_value
# Comment: below a permitted empty `STRUCTURE_SINGLE_BLOCK_IDENTIFIER` which will use all default values
. STRUCTURE_SINGLE_BLOCK_2
___END
```

#### 5.5.1.1. STRUCTURE_SINGLE_BLOCK With Block-Items

In a LCONF-Section STRUCTURE_SINGLE_BLOCK-Items MAY be defined in any order and are NOT REQUIRED to follow the
implemented order of a corresponding LCONF-Schema.

```text
. Color_Block_1
    color :: Blue
    - rgb_list
        157
        174
        235
    menu :: false
```

```text
# The Block could also be defined as
. Color_Block_1
    menu :: false
    - rgb_list
        157
        174
        235
    color :: Blue
```

```text
# The Block could also define only some of the Block-Items: depending on their requirement settings.
#  all other implmented Block-Items would have there implemented default values.
. Color STRUCTURE_SINGLE_BLOCK
    - rgb_list
        157
        174
        235
```

## 5.6. STRUCTURE_NAMED_BLOCKS

 A collection of repeated named STRUCTURE_SINGLE_BLOCKs. A LCONF-Library MUST implement an option to loop over the
collection in order. BLOCKS-Items use one additional LCONF-Indentation-Per-Level and each Block-Item MUST start on a
separate line. All Block-Items MUST be STRUCTURE_SINGLE_BLOCKs with unique names.

A STRUCTURE_NAMED_BLOCKS_IDENTIFIER-Line MUST

* start with a STRUCTURE_NAMED_BLOCKS_IDENTIFIER
* followed by one LCONF_SPACE
* followed by a LCONF-Key-Name

```text
* Color
```

Block-Items are separate lines and use one additional LCONF-Indentation-Per-Level.
In the example below these are: `. BlueBlock, . RedBlock`.

```text
* Color
    . BlueBlock
        Name :: Blue
        - rgb_list
            157
            174
            235
        menu :: NOTSET
    . RedBlock
        Name :: Red
        - rgb_list
            227
            155
            155
        menu :: on
```

A LCONF-Schema-Definition MUST implement one reference STRUCTURE_SINGLE_BLOCK with default values
(TEMPLATE_BLOCK) which will be used as the base for each defined Repeated-Block-Item.

Any number of Block-Items can be defined but this can also be limited in a LCONF-Schema-Definition. e.g.:

* LCONF_NUMBER_MIN_REQUIRED_BLOCKS
* LCONF_NUMBER_MAX_ALLOWED_BLOCKS

### 5.6.1. Default STRUCTURE_NAMED_BLOCKS

The Default STRUCTURE_NAMED_BLOCKS is always an empty collection.

It is PERMITTED to include an Empty STRUCTURE_NAMED_BLOCKS which will keep the default value as implemented in a
LCONF-Schema-Definition (which is always an empty collection). This is the same as if one does not define the
STRUCTURE_NAMED_BLOCKS in a LCONF-Section.

In some cases it might be useful to include an empty STRUCTURE_NAMED_BLOCKS: e.g. if one wants previous comment lines.

Empty STRUCTURE_NAMED_BLOCKS MUST only define the STRUCTURE_NAMED_BLOCKS_IDENTIFIER-Line and MUST NOT set any
LCONF-Block-Item-Lines.

```text
* Color
```

```text
___SECTION :: 4 :: LCONF :: SectionName
* RepeatedBlock_Identifier_name
    # Comment: Named STRUCTURE_SINGLE_BLOCK
    . single_block_item3_key1
        single_block_item1_key :: single_block_item1_value
    . single_block_item3_key2
        single_block_item1_key :: single_block_item1_value
# Comment: below a permitted empty `Repeated-Block-Identifier` which will use the default value
#          which is always an empty collection.
* RepeatedBlock_2
___END
```

#### 5.6.1.1. LCONF_SINGLE_BLOCK_REUSE

Only in a STRUCTURE_NAMED_BLOCKS collection (sequence) one MAY use LCONF_SINGLE_BLOCK_REUSE to assign the settings
of a previous named STRUCTURE_SINGLE_BLOCK to a new named STRUCTURE_SINGLE_BLOCK.

```text
. New_STRUCTURE_SINGLE_BLOCK-Key-Name LCONF_SINGLE_BLOCK_REUSE Other_STRUCTURE_SINGLE_BLOCK-Key-Name

. New_STRUCTURE_SINGLE_BLOCK-Key-Name == Other_STRUCTURE_SINGLE_BLOCK-Key-Name
```

Example of a LCONF_SINGLE_BLOCK_REUSE.

```text
* RepeatedBlock
    . STRUCTURE_SINGLE_BLOCK_Key_Name_1
        FirstName :: Mary
        LastName :: Jackson
        Age :: 35
        Street :: 768 5th Ave # 1332
        City :: New York
        State :: NY
        ZIPCode :: 10019
    # Comment: the second Block will have the same `Last Name, Street, City, State and ZIPCode as the first Block`
    . STRUCTURE_SINGLE_BLOCK Key-Name 2 == STRUCTURE_SINGLE_BLOCK Key-Name 1
        FirstName :: Tim
        Age :: 38
```

## 5.7. STRUCTURE_UNNAMED_BLOCKS

 A collection of repeated unnamed STRUCTURE_SINGLE_BLOCKs. A LCONF-Library MUST implement an option to loop over the
collection in order. BLOCKS-Items use one additional LCONF-Indentation-Per-Level and each Block-Item MUST start on a
separate line.

A STRUCTURE_UNNAMED_BLOCKS_IDENTIFIER-Line MUST

* start with a STRUCTURE_UNNAMED_BLOCKS_IDENTIFIER
* followed by one LCONF_SPACE
* followed by a LCONF-Key-Name

```text
* Color
```

An STRUCTURE_UNNAMED_BLOCKS MUST use as STRUCTURE_SINGLE_BLOCK-Identifier-Line:

* **only a STRUCTURE_SINGLE_BLOCK-Identifier**

```text
* RepeatedBlock
    .
        FirstName :: Mary
    .
        FirstName :: Tom
    .
        FirstName :: Dora
```

Block-Items are separate lines and use one additional LCONF-Indentation-Per-Level.

```text
* Color
    .
        Name :: Blue
        - rgb_list
            157
            174
            235
        menu :: NOTSET
    .
        Name :: Red
        - rgb_list
            227
            155
            155
        menu :: on
```

A LCONF-Schema-Definition MUST implement one reference STRUCTURE_SINGLE_BLOCK with default values
(TEMPLATE_BLOCK) which will be used as the base for each defined Repeated-Block-Item.

Any number of Block-Items can be defined but this can also be limited in a LCONF-Schema-Definition. e.g.:

* LCONF_NUMBER_MIN_REQUIRED_BLOCKS
* LCONF_NUMBER_MAX_ALLOWED_BLOCKS

### 5.7.1. Default STRUCTURE_UNNAMED_BLOCKS

The Default STRUCTURE_UNNAMED_BLOCKS is always an empty collection.

It is PERMITTED to include an Empty STRUCTURE_UNNAMED_BLOCKS which will keep the default value as implemented in a
LCONF-Schema-Definition (which is always an empty collection). This is the same as if one does not define the
STRUCTURE_UNNAMED_BLOCKS in a LCONF-Section.

In some cases it might be useful to include an empty STRUCTURE_UNNAMED_BLOCKS: e.g. if one wants previous comment lines.

Empty STRUCTURE_UNNAMED_BLOCKS MUST only define the STRUCTURE_UNNAMED_BLOCKS_IDENTIFIER-Line and MUST NOT set any
LCONF-Block-Item-Lines.

```text
* Color
```

```text
___SECTION :: 4 :: LCONF :: SectionName
* RepeatedBlock_Identifier_name
    # Comment: UnNamed STRUCTURE_SINGLE_BLOCK
    .
        single_block_item1_key :: single_block_item1_value
    .
        single_block_item1_key :: single_block_item1_value
# Comment: below a permitted empty `STRUCTURE_UNNAMED_BLOCKS_IDENTIFIER-Line` which will use the default value
#          which is always an empty collection.
* RepeatedBlock_2
___END
```
