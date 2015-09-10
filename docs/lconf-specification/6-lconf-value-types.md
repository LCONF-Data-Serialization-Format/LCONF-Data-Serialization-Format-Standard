<div align="center">
    <a href="http://lconf-data-serialization-format.github.io/">
        <img src="../../images/lconf-logo.png" alt="LCONF LOGO" title="The LCONF-Data-Serialization-Format Organization">
    </a>
</div>

> This file is part of the *LCONF-Data-Serialization-Format-Standard Documentation*.
>
> Copyright (c) 2014 - 2015, **peter1000**  <https://github.com/peter1000>.

# 6. LCONF-Value-Types

The set of six main value types includes NOTSET, String, Boolean, Number, Date & Time and Range.

* **NOTSET**

    * TYPE_NOTSET

* **String**

    * TYPE_STRING
    * TYPE_DIGITS
    * TYPE_PATTERN_DIGITS

* **Boolean**

    * TYPE_BOOLEAN

* **Number**

    * TYPE_INTEGER
    * TYPE_FLOAT
    * TYPE_NUMBER

* **Date & Time**

    * TYPE_MONTH
    * TYPE_DAY
    * TYPE_MINUTE
    * TYPE_SECOND
    * TYPE_SECOND_FRACTION
    * TYPE_DAY_MINUTE1
    * TYPE_DAY_MINUTE2
    * TYPE_DAY_SECOND1
    * TYPE_DAY_SECOND2
    * TYPE_DAY_SECOND_FRACTION1
    * TYPE_DAY_SECOND_FRACTION2

* **Range**

    * TYPE_RANGE_OF_ELEMENTS
    * TYPE_RANGE_BY_END_VALUE

**When Is A LCONF-Value Interpreted As One Of The LCONF-Value-Types ?**

Only the corresponding LCONF-Schema-Definition determinates which LCONF-Value is interpreted as which LCONF-Value-Type.

## 6.1. TYPE_NOTSET

A TYPE_NOTSET Value  is the Literal Name Token `NOTSET` and is used to indicate the lack of a value and is different
from an Empty-Value. In a LCONF-Section it is the same as if that item was not specified at all and the implemented
default value will stay assigned.

* `NOTSET` is a reserved LCONF character sequence.

**The main difference between an Empty-Value and TYPE_NOTSET is:**

* A TYPE_NOTSET Value (LCONF_NOTSET) is used to indicate the lack of a value. (e.g. STRUCTURE_TABLE a missing
    Column-Value)

* An Empty-Value is an actual value: e.g an empty string, an empty sequence etc.

Only simple LCONF-Structures MAY contain TYPE_NOTSET values:

* STRUCTURE_PAIR
* STRUCTURE_TABLE: only individual Column-Values MAY be set to TYPE_NOTSET
* STRUCTURE_LIST: only individual List-Values MAY be set to TYPE_NOTSET

It is most useful in STRUCTURE_TABLEs if there are missing data.

```text
___SECTION :: 4 :: LCONF :: example Value Not Set

first :: John
last :: Doe
age :: NOTSET

| people_table
    # name  | height_cm | weight_kg | age    |
    | Tim   | 178       | 86        | 37     |
    | Paula | 156       | NOTSET    | NOTSET |
    | John  | 186       | 84        | 23     |
    | Dora  | 173       | NOTSET    | 45     |

- color_name_list
    Red
    Blue
    NOTSET
    Green

- color_name_list :: Red, Blue, NOTSET, Green
___END
```

LCONF-Template-Structure usually will implement a TYPE_NOTSET Values as None, nothing, void.

TYPE_NOTSET is also special that it can be used in a LCONF-Section in place of any other expected Value Type.
**NOTE**: A LCONF-Schema-Definition can define that an item is required: which means it can not be set to TYPE_NOTSET.

## 6.2. String

There a three subtypes of Strings.

### 6.2.1 TYPE_STRING

A sequence of zero or more Unicode characters. It never spans multiple lines.

```text
key1 :: This is a string value of a STRUCTURE_PAIR.

- example_list
    This is a string value of a STRUCTURE_LIST.
    This is another string value of the same STRUCTURE_LIST.
```

NOTE: LCONF-Schema-Definition can further constrain TYPE_STRING e.g: require a minimum and/or maximum number of chars.

### 6.2.1 TYPE_DIGITS

A TYPE_STRING constraint to only LCONF_DIGITS (zero or more LCONF_DIGITS).
These can be used for very long Digit sequences for e.g Unique Identity Numbers which will be kept as string values.

```text
key1 :: 19453841344987531223565469
```

NOTE: LCONF-Schema-Definition can further constrain TYPE_DIGITS e.g: require a minimum and/or maximum number of chars.

### 6.2.2 TYPE_PATTERN_DIGITS

A TYPE_STRING constraint to a pattern where only the individual LCONF_DIGITS MAY change. (zero or more chars).
These can be used for common pattern for e.g Social Security number which will be kept as string values.

*TYPE_PATTERN_DIGITS* format uses the LCONF_AT_SIGN `@` as placeholder for expected digits.

**1. Example: the LCONF-DateTime type could be also represented by a TYPE_PATTERN_DIGITS**

`@@@@-@@-@@T@@:@@:@@`

```text
2013-07-01T12:30:59
```

`@@@@-@@-@@ @@:@@:@@`

```text
2013-07-01 12:30:59
```

**2. Example: Social Security number**

`AAA-GG-SSSS` could be a pattern `@@@-@@-@@@@`

```text
123-55-6678
```

## 6.3. TYPE_BOOLEAN

LCONF_TRUE or LCONF_FALSE.  Literal Name Token: `true` or `false`.

```text
item1 :: true

item2 :: false
```

A LCONF-Template-Structure usually will implement an Empty TYPE_BOOLEAN Values as TYPE_NOTSET.

## 6.4. Numbers

### 6.4.1. TYPE_INTEGER

A TYPE_INTEGER MUST contain only LCONF_DIGITS. It MAY have a preceding LCONF_PLUS or LCONF_MINUS.

* 64 bit (signed long) range expected (-9223372036854775808 to +9223372036854775807).

```text
key1 :: 89

- example_list
    +18950
    0
    -800000
```

LCONF-Template-Structure usually will implement an Empty LCONF-Integer Values as TYPE_NOTSET or a predefined
Integer (e.g. zero)

NOTE: LCONF-Schema-Definition can further constrain TYPE_INTEGER e.g: require a minimum and/or maximum value.

### 6.4.2. TYPE_FLOAT

TYPE_FLOAT supports four different notations:

* Fractional: `+3.1415`, `-3.1415`
* Exponent: `5e+22`, `-2E-2`
* Fractional And Exponent Mixed: `6.196E63`, `-1.54e-003`
* Fraction P/Q Of Two Integers: `+3/4`, `-93/16`, `1/8`, `2789/-598`

LCONF-Template-Structure usually will implement an Empty TYPE_FLOAT as TYPE_NOTSET or a predefined Float (e.g. 0.0).

NOTE: LCONF-Schema-Definition can further constrain TYPE_FLOAT e.g: require a minimum and/or maximum value.

#### 6.4.2.1. Fractional Notation

A TYPE_INTEGER part followed by a LCONF-Number-Fractional-Part.

```text
key1 :: +1.0

- example_list
    -0.01
    3.1415
```

#### 6.4.2.2. Exponent Notation

A TYPE_INTEGER part followed by a LCONF-Number-Exponent-Part.

```text
key1 :: 5e+22

- example_list
    -1e6
    -2E-2
```

#### 6.4.2.3. Fractional And Exponent Mixed Notation

A TYPE_INTEGER part followed by a LCONF-Number-Fractional-Part followed by a LCONF-Number-Exponent-Part.

```text
key1 :: 6.196E63

- example_list
    -1.54e-003
    -1.54e+003
    2.5e-4
```

#### 6.4.2.4. Fraction P/Q Of Two Integers Notation

A TYPE_INTEGER part followed by a LCONF_SLASH followed by a TYPE_INTEGER part which MUST NOT be equal to 0 (zero).

```text
key1 :: +3/4

- example_list
    -93/16
    1/8
    2789/-598
```

### 6.4.3. TYPE_NUMBER

A TYPE_NUMBER value can either be a TYPE_INTEGER or TYPE_FLOAT.

```text
- example_valid_number_list
    -93/16
    1/8
    2789/-598
    -1.54e-003
    -1.54e+003
    2.5e-4
    5
    -189556
    +23
    -0.01
    3.1415
```

NOTE: LCONF-Schema-Definition can further constrain TYPE_FLOAT e.g: require a minimum and/or maximum value.

## 6.5. Date & Time

A LCONF Date & Time value is based on the ISO 8601 standard and supports following notations.

A LCONF-DateTime value is a combination of a LCONF-Date and LCONF-Time and has the LCONF-Date-Time-Separator between
the Date and Time part.

LCONF-Template-Structure usually will implement an Empty LCONF Date & Time Value as TYPE_NOTSET or a predefined
Date & Time Value (e.g. 1970-01, 1970-01-01, 00:00:00, 1970-01-01T00:00:00, 1970-01-01 00:00:00).

### 6.5.1. TYPE_MONTH

`YYYY-MM`:

* A LCONF-Year
* followed by a LCONF-Date-Separator
* followed by a LCONF-Month

```text
1932-08

2015-01
```

### 6.5.2 TYPE_DAY

`YYYY-MM-DD`:

* A LCONF-Year
* followed by a LCONF-Date-Separator
* followed by a LCONF-Month
* followed by a LCONF-Date-Separator
* followed by a LCONF-Day

```text
1932-08-31

2014-11-15
```

### 6.5.3. TYPE_MINUTE

`hh:mm`:

* A LCONF-Hour
* followed by a LCONF-Time-Separator
* followed by a LCONF-Minute

```text
12:30

23:59
```

### 6.5.4. TYPE_SECOND

`hh:mm:ss`:

* A LCONF-Hour
* followed by a LCONF-Time-Separator
* followed by a LCONF-Minute
* followed by a LCONF-Time-Separator
* followed by a LCONF-Second

```text
02:30:42

19:00:02
```

### 6.5.5. TYPE_SECOND_FRACTION

`hh:mm:ss.fff`:

* A LCONF-Hour
* followed by a LCONF-Time-Separator
* followed by a LCONF-Minute
* followed by a LCONF-Time-Separator
* followed by a LCONF-Second
* followed by a LCONF-Second-Fraction-Part

```text
StartFractionSecond :: 12:30:59.001

StartFractionSecond :: 18:53:16.1

StartFractionSecond :: 04:02:00.000156
```

### 6.5.6. TYPE_DAY_MINUTE1

`YYYY-MM-DD hh:mm`

```text
2013-07-01 12:30
```

### 6.5.7. TYPE_DAY_MINUTE2

`YYYY-MM-DDThh:mm`

```text
2013-07-01T12:30
```

### 6.5.8. TYPE_DAY_SECOND1

`YYYY-MM-DD hh:mm:ss`

```text
2013-07-01 12:30:59
```

### 6.5.9. TYPE_DAY_SECOND2

`YYYY-MM-DDThh:mm:ss`

```text
2013-07-01T12:30:59
```

### 6.5.10. TYPE_DAY_SECOND_FRACTION1

`YYYY-MM-DD hh:mm:ss.fff`

```text
2013-07-01 12:30:59.001

2013-07-01 18:53:16.1

2013-07-01 04:02:00.000156
```

### 6.5.11. TYPE_DAY_SECOND_FRACTION2

`YYYY-MM-DDThh:mm:ss.fff`

```text
2013-07-01T12:30:59.001

2013-07-01T18:53:16.1

2013-07-01T04:02:00.000156
```

## 6.6. Range

A range defines an arithmetic sequence where the first element is the LCONF-Range-Start-Number.

LCONF-Template-Structure usually will implement an Empty LCONF-Range Value as TYPE_NOTSET or a predefined
LCONF-Range Value.

LCONF supports two types of ranges.

### 6.6.1. TYPE_RANGE_OF_ELEMENTS

A TYPE_RANGE_OF_ELEMENTS refers to an arithmetic sequence which contains as many elements as defined by the
LCONF-Integer part of the LCONF-Range-Number-Of-Elements.

A TYPE_RANGE_OF_ELEMENTS MUST:

* start with a LCONF-Range-Start-Number
* followed by a LCONF-Range-Part-Separator
* followed by a LCONF-Range-Step-Number
* followed by a LCONF-Range-Part-Separator
* followed by LCONF-Range-Number-Of-Elements

 ```text
 LCONF-Range-Start-Number|LCONF-Range-Step-Number|LCONF-Range-Number-Of-Elements
 ```

```text
-10|1|*21

512.4|0.125|*8
```

* `-10|1|*21` represents a sequence of:

    `-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10`

* `512.4|0.125|*8` represents a sequence of:

    `512.4, 512.275, 512.15, 512.025, 511.9, 511.775, 511.65, 511.525`

### 6.6.2. TYPE_RANGE_BY_END_VALUE

A TYPE_RANGE_BY_END_VALUE defines an arithmetic sequence where the first element is the LCONF-Range-Start-Number.

A TYPE_RANGE_BY_END_VALUE supports following notations:

* `LCONF-Range-Start-Number|LCONF-Range-Step-Number|LCONF-Range-End-Number`
* `LCONF-Range-Start-Number|LCONF-Range-Step-Number|LCONF-Range-End-Number|FORCE`

    To force always the inclusion of the *LCONF-Range-End-Number*.

A TYPE_RANGE_BY_END_VALUE MUST:

* starts with a LCONF-Range-Start-Number
* followed by a LCONF-Range-Part-Separator
* followed by a LCONF-Range-Step-Number
* followed by a LCONF-Range-Part-Separator
* followed by LCONF-Range-End-Number
* followed optionally by a LCONF-Range-Part-Separator and LCONF_FORCE.

#### 6.6.2.1. Positive LCONF-Range-Step-Number

If the LCONF-Range-Step-Number value is greater than zero then the LCONF-Range-Start-Number MUST be less or equal to
the *LCONF-Range-End-Number*.

**The last element is the largest**: `Start-Number + i * Step-Number less or equal to LCONF-Range-End-Number`.

```text
-10|1|5

-10|1|5|FORCE

100.8|1.27|106

100.8|1.27|106|FORCE
```

* `-10|1|5` represents a sequence of:

    `-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5`

* `-10|1|5|FORCE` represents a sequence of:

    `-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5`

* `100.8|1.27|106` represents a sequence of:

    `100.8, 102.07, 103.34, 104.61, 105.88`

* `100.8|1.27|106|FORCE` represents a sequence of:

    `100.8, 102.07, 103.34, 104.61, 105.88, 106`

#### 6.6.2.2. Negative LCONF-Range-Step-Number

If the LCONF-Range-Step-Number value is less than zero then the LCONF-Range-Start-Number MUST be greater or equal to
the *LCONF-Range-End-Number*.

**The last element is the smallest**: `Start-Number + i * Step-Number greater or equal to LCONF-Range-End-Number`.

```text
10|1|-5

10|1|-5|FORCE

100.8|-1.27|92.1

100.8|-1.27|92.1|FORCE
```

* `10|1|-5` represents a sequence of:

    `10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0, -1, -2, -3, -4, -5`

* `10|1|-5|FORCE` represents a sequence of:

    `10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0, -1, -2, -3, -4, -5`

* `100.8|-1.27|92.1` represents a sequence of:

    `100.8, 99.53, 98.26, 96.99, 95.72, 94.45, 93.18`

* `100.8|-1.27|92.1|FORCE` represents a sequence of:

    `100.8, 99.53, 98.26, 96.99, 95.72, 94.45, 93.18, 92.1`
