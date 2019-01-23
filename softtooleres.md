# Software Tools for eResearchers

# Contents

# Regular Expressions

## What Are Regular Expressions?

Regular Expressions ("RegEx") are general character pattern notations that provides the means for efficient and effective text processing. The main use is search, replace, and validation functions, which can - at least in part - be achieved through a variety of text editors. However, compared to various RegEx tools, using a text editor is a fairly labour intensive process. For researchers, especially those whose work involves a great deal of data extraction and manipulation, having a good grasp of regular expressions is one of the most powerful tools available.

The origins of regular expressions come from a combination of mathematics, linguistics, and computer science. The first formal development was by Stephen Cole Kleen who described a mathematical "regular language" in 1951 that would be located as a Type-3 grammer in the hierarchy developed by Noam Chomsky in 1956, which a finite-state machine will recognise.

> Fun fact. Stephen Cole Kleene also wrote an alternative proof to Gödel's incompleteness theorems that were easier to understand. From this it has been quipped that "Kleeneness is next to Gödelness".   

The first major computational use was in 1968 through pattern matching in the QED ("quick editor") text editor. QED was originally written in 1965 and was designed for teleprinter usage. When Ken Thompson wrote a version for the Compatible Time-Sharing System (CTSS) in 1968 a level of regular expressions were introduced. At around the same researchers, including Douglas T. Ross, implemented a regular expression engine in compiler design.

The regular expression system in QED was ported to `ed` in 1969 to become a foundation of the UNIX operating system. Within `ed` the function for searches was `g/re/p`, "global search for regular expreesion and print (to standard output). The functionality was considered so useful that Ken Thompson used the logic from `ed` and wrote the first version of`grep` overnight in November 1974.

Apart from `grep`, other major applications in the regular expression wolrd include `sed` ("stream editor", 1974), `awk` (named after its authors "Aho, Weinberger, Kernighan", a data-driven scripting language, first release 1977), `perl` (often given the common backronym "Practical Extraction and Reporting Language", first released 1987). Many other programming languages use regular expression engines, but the aforementioned are considered the main foundations.

## Metacharacters

A major feature of RegEx is the use of *metacharacters*, which have a special meaning to the RegEx application over and above their literal meaning. For example the use of a `^` to represent the beginning of a line, or a `*` to represent any of 0 or more characters. 

The POSIX standard (basic and extended) has some 14 metacharacters. In this standard metacharacters must be "escaped" (preceded by the backslash character `\`) to be considered in their literal meaning. Famously, this includes the backslash character itself, so in order to express a literal backslash one requires `\\`.  The main difference between POSIX Basic Regular Syntax (BRE) and Extended Regular Syntax (SRE) is that the basic form requires the normal and curled bracket characters to also be escaped i.e., `\(\)` and `\{\}`.

The entire suite of metachaarcters that require an escape for their literal include opening and closing normal, and curled brackets, the caret, the dollar sign, the period, the bar, question mark, asterisk, the plus sign, and the backslash i.e. "(", "{" "}", ")", "^", "$", ".", "|", "?", "*", "+", "\". 

In addition to these metacharacters there are also metacharacters that are not escaped by the backslash. This includes the aforementioned normal and curled bracket characters and the `-` character which is used to indicated ranges within a square-bracket set.

Metacharacters are so important to understanding regular expressions and so common across the core applications it is worth establishing a thorough understanding of them early. The following outlines the major metacharacters, their meaning, and an example using the search utility `grep`. Note that a metacharacter can have different meanings *within* regular expressions. The caret symbol, can either stands as beginning of a line anchor *and* a negation symbol in a set range. 

| Metacharacter | Explanation         | Example                                          |
|:--------------|:-------------------|:-----------------------------------------------|
| ^             | Beginning of line anchor   | `grep '^row' /usr/share/dict/words`       |
| $             | End of line anchor         | `grep 'row$' /usr/share/dict/words`       |
| .             | Any single character       | `grep '^...row...$' /usr/share/dict/words` |
| *             | Match zero plus preceding characters | `grep '^...row.*' /usr/share/dict/words`   |
| [ ]           | Matches one in the set                 | `grep '^[Pp].row..$' /usr/share/dict/words`   |
| [x-y]         | Matches on in the range                | `grep '^[s-u].row..$' /usr/share/dict/words`   |
| [^ ]          | Matches one character not in the set   |  `grep '^[^a].row..$' /usr/share/dict/words`   |
| `|`     	| Logical OR   |  `grep '^[^a|P].row..$' /usr/share/dict/words`   |
| [^x-y]        | Matches any character not in the range | `grep '^[^a-z].row..$' /usr/share/dict/words`   |
| \             | Escape a metacharacter |  `grep '^A\.$' /usr/share/dict/words`	|

In addition to singular metacharacters there are also character classes, which take a common range statement. For example the class '[[:alpha:]]' is the equivalent of the range [A-Za-z] and [:punct:]  is equivalent of [][!"#$%&'()*+,./:;<=>?@\^_`{|}~-] . POSIX requires that classes must be expressed within square brackets, which does have the advantage to extend outside the class (e.g., [[:upper::]0] would match with all upper case alphabet characters and the number 0.

| Metacharacter | Explanation                                           |
|---------------|-------------------------------------------------------|
| [:digit:]	| Only the digits 0 to 9                                |
| [:alnum:]	| Alphanumeric characters 0 to 9 OR A to Z or a to z.	| 
| [:alpha:]	| Alpha character A to Z or a to z.                 	|
| [:blank:]	| Space and TAB characters only.                        |
| [:lower:]	| Lower case characters					|
| [:punct:]	| Punctuation characters				|
| [:upper:]	| Upper case characters					|
| [:xdigit:]	| Hexadecimal digits					|

Assuming that the requests are not contradictory metacharacters can be combined. For example, `^$` represents blank lines, `^firstword.*lastword$` would search for a line of text that starts with "firstword" and ends with "lastword" and contains anything in between.

## Searching with `grep`

Most basic use of a regular expression is searching text and the basic tool for this is `grep` which considered sufficiently important that the Oxford English dictionary has included entries for "grep" as both a noun and a verb. One will also find references to `egrep` and `fgrep` which included extended regex syntax and fixed strings. These utilities however have been deprecated and their funcationality is included in the command switches `-E` and `-F` respectively.

A typical formal structure is a search for a pattern with Boolean logic, grouping, quantification (e.g., number of instances), and wildcards, followed by an optional operation (arithemetic count, replacement etc) on the pattern. In a POSIX environment a command structure is `application (options) RegEx file` e.g., `grep -i ATEK gattaca.txt`. The command is invoked, with options, with a regular expression pattern on a particular file. The result is sent to standard output by default.

The usual action of `grep` is to search for a given string in one or more files with the standard syntax of grep "STRING" FILES (e.g., `grep "ELM" gattaca.txt`). 

Metacharacters can be combined in an interesting manner with grep options. For example; (i) using grep to count the number of empty lines in a file; `grep -c '^$' filename` (ii) a search for words with no vowels; `grep -v "[aeiou]" /usr/share/dict/words`, (iii) a case-insensitive search for lines that start with QANTAS, and only QANTAS (not QANTAS1, for example), in all files in the specified directory. `grep -iw "^QANTAS" /usr/local/dict/*` - Antipodeans might be surprised (and disappointed) by this result.

## Editing with `sed`

As the name implies `sed` is a *stream editor*, which means it takes a data stream in a non-interactive mode, makes the changes according to a regular expression, and sends the results to standard output. By default it does not change the original data stream, which is typically a file. It *could* be used to use the data stream source as input from a keyboard, but this would be an unusual use of the application. More typically it is used for massive and rapid search and replaces on a number of documents, various forms of data cleaning (e.g., removing trailing whitespace), or conversion from one type of text markup to another (e.g., from a wiki syntax to html or troff etc).

The general form is `sed [OPTION] [SCRIPT] [INPUT]`. Common options are `e` (multiple scripts per command), `-f` (add script file) and `-i` (in-place editing). For example, `sed 's/^/     /' gattaca.txt` to insert five spaces at the beginning of each line from the input stream of `gattaca.txt` or `sed '/ELM/s/Q/\$/g' gattaca.txt` to substitute all instances of 'Q' with '$' (note escape character) on lines that start with 'ELM'. 

As examples of data-cleaning, `sed /^$/d $filename` will remove all blank lines from a file, whereas 
`sed s/ *$// $filename` will delete all spaces at the end of every line. Another issue, which used to be more common than it is now, is converting MS-Windows text files to *nix formats and vice versa. To translated *nix files to MS-Windows files a carriage return needs to be added to the end of each line, whereas to translate from MS-Windows to *nix, the carriage return marker needs to be removed. The two commands to do this are `sed s/$/\r/g' $filename`, and `sed 's/\r$//g' $filename` respectively.

"The sed $HOME" (`http://sed.sourceforge.net/`). In particular there is a popular list one-line sed commands (`http://sed.sourceforge.net/sed1line.txt`)

## Reporting Structured Data with `awk`

As a data driven programming language `awk` is particularly good at understanding and manipulating text structured by fields - such as tables of rows and columns. Unlike `grep`, which searches for strings from an input, or `sed`, which carries out editing on the same, `awk` offers more computational options. The organisation of an `awk` program follows the form: pattern { action }. This is sometimes structured with BEGIN and END which specify actions to be taken before any lines are read, and after the last line is read. 

The easiest and certainly one of the most common uses of awk is create reports from structured data.
Columns and rows are referenced by number (e.g., `print $1` for the first column, `NR>1` for all rows greater than 1) and by default the space acts as the delimiter. To modify this use `awk -F,` (or `awk -F ','`) to use, for example, the comma as the delimiter. Other shell commands can be included in `awk`. 

To give several examples of this:

`awk -F"," '{print $3}' quakes.csv`
`awk -F"," '{print $1 " : " $3}' quakes.csv`
`awk -F"," '{print $1 " : " $3 | "sort"}' quakes.csv | less`
`awk -F"," 'END {print NR}' quakes.csv`    
`awk -F"," 'NR>1{print $3 "," $2 "," $1}' quakes.csv`   
`awk -F"," '(NR <2) || (NR!=6) && (NR<9)' quakes.csv > selection.txt`   

Other useful awk one-liners make use of the arithmetic functions of this programming language, such as printing  the total number of fields in $file, or printing the sum of the fields (columns) of every line (row); NF is number of field. For example:

`awk '{totalf = totalf + NF }; END {print totalf}' $file`
`awk '{sum=0; for (i=1; i<=NF; i++) sum=sum+$i; print sum}' $file`    
[EDIT]

Thus `awk`, in addition to reporting information, can also use arithmetic operators with the structured data and execute and process shell commands. Later examples will be given where variables, loops, and conditionals are applied along with functions and multiple input streams.

A collection of basic `awk` one-liners are at: `http://www.pement.org/awk/awk1line.txt`.

## Basic and Extended Regular Expressions



# Credits

## Images

J. Finkelstein, "Set Inclusions Described by the Chomsky Hiearchy", 2010

## Books

Jeffrey E. F. Friedl, Mastering Regular Expressions (third edition), O’Reilly Media, 2007 FP 1997 
