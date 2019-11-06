# CUINSPACE -- C CODING STANDARDS

```
"Indeed, the ratio of time spent reading versus writing is well over 10 to 1.
We are constantly reading old code as part of the effort to write new
code. ...[Therefore,] making it easy to read makes it easier to write."

    -- Robert C. Martin
```

The purpose of these coding standards is to promote consistency, good style, and
code readability. Follow these standards to make it easier for everyone working
on your code now and in the future to understand it and make modifications.

## File Paths and Names:

Attempt to keep file paths as succinct as possible. Long file paths are annoying
and make it difficult to fit on a single line. However, only do this within
reason. Never make a file path too short as to clutter unrelated things together
and never name files in a way that would make their purpose ambiguous.

* **GOOD:** `/home/john/src/modules/comms/comms.h`
* **BAD:**  `/home/john/src/mods/com/com.h`
* **BAD:**  `/home/john/src/modules/communications/headers/main/communications.h`

## Naming Conventions:

Variables (and functions) should be named as descriptively as is reasonable.
Single or few-letter naming should not be used unless the variable is temporary
and limited in scope such as in a `for` loop. Variables should be named such
that their purpose is unambiguous at first glance. The names of files,
functions, and variables are forms of documentation in your code. Optimize for
readability.

Variables should be named using `camelCase` whereas functions and structs should
be named using `snake_case`. This helps to easily differentiate between a
variable and a function/struct type at first glance.

Type constructs (e.g. structs) should be defined with the suffix `_t` as in
`myType_t`.

Global variables should be defined with the suffix `_g` as in `globalVar_g`

Functions should have a prefix indicating which module they are a part of. For
example: `int sd_write_block(...)` if the function is part of the SD card
driver.

Generally do not `typedef` unions and structures. Seeing `struct` helps to
identify the variable as a structure and seeing `union` helps to identify the
variable as a union. Hiding this behind a typedef means you need to write fewer
characters but it also increases the amount of pre-requisite knowledge that
someone needs in order to understand your code.

Files should be named using `kebab-case` because it looks pretty.

* **GOOD:** `int altimeter_get_height(void){...}`
* **GOOD:** `struct packet_struct_t {...};`
* **GOOD:** `double rocketHeight = altimeter_get_height();`
* **GOOD:** `initialize-peripherals.c`
* **BAD:**  `int getH(void){...}`
* **BAD:**  `struct pst PStruct{...};`
* **BAD:**  `double x = get_h();`
* **BAD:**  `InitPeripherals.c`

## Declaring/Assigning Variables:

Do not declare multiple variables on the same line or in one declaration which
spans multiple lines. Do not assign values to multiple variables on the same
line or in one statement spanning multiple lines.

* **GOOD:**
```
int foo;
int bar;
```

* **BAD:**
```
int foo, bar = 50;
```

* **BAD:**
```
int foo,
bar;
```

## Macros:

Macros (defines and enums) should be written in uppercase snake case. This helps
to differentiate them from both variables and functions:

* **GOOD:** `#define MAX_LENGTH 1024`
* **BAD:**  `#define maxLength 2014`

## Line Length:

Lines of code should be limited to 80 characters long. This ensures that no
single line is too long and helps to avoid run-on lines of code. C makes it easy
to split lines of code because of its use of ';' to separate statements. 80
lines is chosen to give the greatest level of compatibility between different
devices (for example, a serial console may only output 80 by 24 columns). It
also means that your code fits nicely within the typical field of vision of a
person.

If arguments to a function don't fit nicely on one line, split them thusly:

```
int function_with_many_args(int firstInteger, char* firstString,
                            int secondInteger, char* secondString)
{
    <function_body>
}
```

## Indentation:

Four spaces of indentation should be used. This is preferable over two spaces
because it makes nested code stand out more, and it is preferable over a tab
character because tab characters may be rendered differently in different
environments.

## Brace Style:

CUIS follows the OTBS (One True Brace Style). It is a variant on the K&R style
where braces are on their own line for functions, on the same line as control
statements, and are on every control statement no matter how many lines they
contain:

As a point of convenience, an `else` or `else if` should be on the line below
the closing brace of the above control statement as this makes it easy to select
the entire block of code that the statement occupies if it needs to be cut,
without also deleting the closing brace of the `if` statement above it.

```
int function(int arg1, char* arg2)
{
    if (arg1 > MAX_LENGTH) {
        <multiple>
        <different>
        <statements>
    }
    else {
        <single_statement>
    }
}
```

## Ternary Operator Conventions:

Use of the ternary operator should be limited to simple statements with no
complex logic:

* **GOOD:** `x = (y == 10) ? 2 : 5;`
* **BAD:**  `x = (yStruct->size == fillRect(20,50,10)) ? yStruct->size+3 : 5;`)

## Goto Conventions:

`goto`s should be avoided at (almost) all cost. While they are essential in
Assembly programming, in C they make the code far harder to follow. In cases
where a `goto` must be used, it should only ever be used in error handling and
should only be used to jump within the same function. Optimize for readability.

## Spacing Conventions:

There should always be a space before and after a binary operator:

* **GOOD:** `z = x + y;`
* **BAD:**  `z=x+y;`

There should never be a space around unary operators:

* **GOOD:** `z++;`
* **BAD:** `z ++;`

## Order of Include Statements:

Include statements should start with the most specific header for the current
file and should get more general in scope:

    * First include the header that is related to the current file.
    * Then include the headers related to the same component or module.
    * Then include the headers related to other components or modules.
    * Lastly include other libraries such as system libraries (e.g. stdio.h).

```
// File i2c.c
#include <i2c.h>
#include <packet.h>
#include <communications.h>
#include <monitoring.h>
#include <stdio.h>
```

## Commenting Conventions:

Comments within your code should aim to be as descriptive as possible. They
should typically explain _what_ the code is doing not _how_ or _why_ the code is
doing it (those are left for external documentation).

Comments should never state the obvious. Assume that the reader of the program
has at least a basic understanding of programming (however do **not** assume
that they have a basic understanding of _your_ program).

Doxygen is used to automatically generate a documentation skeleton, any comments
which you intend to be included in this documentation should start with /\*\*.
Single line comments (`//`) will never be included.

Every function should be preceded by a comment block explaining its purpose,
arguments, and return values as such:
```
/**
 * @fn static inline void start_next_transaction (void)
 * @brief Starts the next queued transaction if there is one and there is
 *  no currently active transaction. This function is inline so that is can be
 *  safely called from an ISR.
 * [One or more @param directives describing input parameters]
 * [One @return directive describing what is returned]
 */
static inline void start_next_transaction (void)
{
    ...
}
```

The `@xxxxx` directives are used so that the Doxygen documentation generator
program can recognize and include this information in automatically generated
documentation.

Furthermore, important variables or odd looking or odd feeling lines or blocks
of code should be accompanied by comments preceding them which explain their
purpose and help to clarify their function. Optimize for readability.

Each file should contain a block of comments at the top which details the name
of the file, a general purpose, the original author, the original date of
creation, the last person to edit the file, and the last date that the file was
edited on. Doxygen style commenting should be used, like so:

```
/**
 * @file File Name
 * @desc <detailed_description_of_file's_purpose>
 * @author <name_of_original_author>
 * @date <original_date_of_creation>
 * Last Author: <name_of_most_recent_author>
 * Last Edited On: <date_of_most_recent_change>
 */
 ```

 It is perfectly okay to put a big block of documentation at the top of the file
 underneath this information if it will be helpful to someone who needs to work
 with the code contained in that file. As an example, see the top of `sd.c`.
 This documentation should contain only essential information about things like
 the overall procedure that the code follows, resources used as references (e.g.
 links to websites, names of books), and any large workarounds or issues with
 the file.

 You can also put a ``TODO:`` directive at the bottom of the top-of-file
 comments which acts as a list of things left to do or improve with the code in
 that file.

 ```
/**
 * TODO: Improve performance of block writes
 *       Refactor send_cmd() to be cleaner with less duplicate code
 *       Add large block write functionality
 */
 ```

Here are some more Doxygen key words which may help you to comment your code:
  `@struct`, `@union`, `@enum`, `@fn`, `@var`, `@def`, `@typedef`, `@file`
