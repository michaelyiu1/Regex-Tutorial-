# Regex Tutorial
This is a tutorial that explains how a specific regular expression, or regex, functions by breaking down each part of the expression and describing what it does

## Summary

A regular expression (also called regex or regexp) is a way to describe a pattern. It is used to locate or validate specific strings or patterns of text in a sentence, document, or any other character input.

Regular expressions use both basic and special characters. Basic characters are standard letters, numbers, and general keyboard characters, while all other characters are considered special.

This article will introduce special characters and discuss using them in regex and extended regular expressions (ERE) with examples.


## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components

### Anchors
Anchors have special meaning in regular expressions. They do not match any character. Instead, they match a position before or after characters:

 ^ – The caret anchor matches the beginning of the text.
 $ – The dollar anchor matches the end of the text.

 See the following example:

let str = 'JavaScript';
console.log(/^J/.test(str));

Output: true

Summary:
Use the ^ anchor to match the beginning of the text.
Use the $ anchor to match the end of the text.
Use the m flag to enable the multiline mode that instructs the ^ and $ anchors to match the beginning and end of the text as well as the beginning and end of the line.

### Quantifiers
Quantifiers specify how many instances of a character, group, or character class must be present in the input for a match to be found.

#### Character Meanings
x*	
Matches the preceding item "x" 0 or more times. For example, /bo*/ matches "boooo" in "A ghost booooed" and "b" in "A bird warbled", but nothing in "A goat grunted".

x+	
Matches the preceding item "x" 1 or more times. Equivalent to {1,}. For example, /a+/ matches the "a" in "candy" and all the "a"'s in "caaaaaaandy".

x?	
Matches the preceding item "x" 0 or 1 times. For example, /e?le?/ matches the "el" in "angel" and the "le" in "angle."

If used immediately after any of the quantifiers *, +, ?, or {}, makes the quantifier non-greedy (matching the minimum number of times), as opposed to the default, which is greedy (matching the maximum number of times).

x{n}	
Where "n" is a positive integer, matches exactly "n" occurrences of the preceding item "x". For example, /a{2}/ doesn't match the "a" in "candy", but it matches all of the "a"'s in "caandy", and the first two "a"'s in "caaandy".

x{n,}	
Where "n" is a positive integer, matches at least "n" occurrences of the preceding item "x". For example, /a{2,}/ doesn't match the "a" in "candy", but matches all of the a's in "caandy" and in "caaaaaaandy".

x{n,m}	
Where "n" is 0 or a positive integer, "m" is a positive integer, and m > n, matches at least "n" and at most "m" occurrences of the preceding item "x". For example, /a{1,3}/ matches nothing in "cndy", the "a" in "candy", the two "a"'s in "caandy", and the first three "a"'s in "caaaaaaandy". Notice that when matching "caaaaaaandy", the match is "aaa", even though the original string had more "a"s in it.

x*?
x+?
x??
x{n}?
x{n,}?
x{n,m}?

By default quantifiers like * and + are "greedy", meaning that they try to match as much of the string as possible. The ? character after the quantifier makes the quantifier "non-greedy": meaning that it will stop as soon as it finds a match. For example, given a string like "some <foo> <bar> new </bar> </foo> thing":

/<.*>/ will match "<foo> <bar> new </bar> </foo>"
/<.*?>/ will match "<foo>"

#### Example
Repeated pattern
const wordEndingWithAs = /\w+a+\b/;
const delicateMessage = "This is Spartaaaaaaa";

console.table(delicateMessage.match(wordEndingWithAs)); // [ "Spartaaaaaaa" ]


### Grouping Constructs
Groups group multiple patterns as a whole, and capturing groups provide extra submatch information when using a regular expression pattern to match against a string. Backreferences refer to a previously captured group in the same regular expression.

#### Types
(x)	
Capturing group: Matches x and remembers the match. For example, /(foo)/ matches and remembers "foo" in "foo bar".

A regular expression may have multiple capturing groups. In results, matches to capturing groups typically in an array whose members are in the same order as the left parentheses in the capturing group. This is usually just the order of the capturing groups themselves. This becomes important when capturing groups are nested. Matches are accessed using the index of the result's elements ([1], …, [n]) or from the predefined RegExp object's properties ($1, …, $9).

Capturing groups have a performance penalty. If you don't need the matched substring to be recalled, prefer non-capturing parentheses (see below).

String.prototype.match() won't return groups if the /.../g flag is set. However, you can still use String.prototype.matchAll() to get all matches.

(?<Name>x)	
Named capturing group: Matches "x" and stores it on the groups property of the returned matches under the name specified by <Name>. The angle brackets (< and >) are required for group name.

For example, to extract the United States area code from a phone number, we could use /\((?<area>\d\d\d)\)/. The resulting number would appear under matches.groups.area.

(?:x)	Non-capturing group: Matches "x" but does not remember the match. The matched substring cannot be recalled from the resulting array's elements ([1], …, [n]) or from the predefined RegExp object's properties ($1, …, $9).
\n	
Where "n" is a positive integer. A back reference to the last substring matching the n parenthetical in the regular expression (counting left parentheses). For example, /apple(,)\sorange\1/ matches "apple, orange," in "apple, orange, cherry, peach".

\k<Name>	
A back reference to the last substring matching the Named capture group specified by <Name>.

For example, /(?<title>\w+), yes \k<title>/ matches "Sir, yes Sir" in "Do you copy? Sir, yes Sir!".

### Bracket Expressions
A bracket expression (an expression enclosed in square brackets, "[]" ) is an RE that shall match a specific set of single characters, and may match a specific set of multi-character collating elements, based on the non-empty set of list expressions contained in the bracket expression.

Some common string methods you might use with regular expressions are match, replace, and split. The first takes the form str.match(regex) and returns an array of matches or null if none are found. The next looks like str.replace(regex, repl) which returns a new string with repl having replaced whatever was matched by the regex. Finally, split is used to break up a string into an array and is usually just given a delimiter like a space or a comma, but a regex can be used as well.

#### Brackets
Brackets indicate a set of characters to match. Any individual character between the brackets will match, and you can also use a hyphen to define a set.

'elephant'.match(/[abcd]/) // -> matches 'a'
You can use the ^ metacharacter to negate what is between the brackets.

'donkey'.match(/[^abcd]/) // -> matches 'o'

#### Parentheses
Parentheses represent remembered matches. This is especially useful for find-and-replace operations or any time you need to do something with part of the match. When a match is remembered you can use $n to refer to it, starting with $1 up to $9, or with $& to refer to the entire match.

'Firsty McLastname'.match(/([A-Za-z]+)\s([A-Za-z]+)/) // -> matches 'First McLastname' with 'Firsty' remembered as $1 and 'McLastname' as $2

'Firsty McLastname'.replace(/([A-Za-z]+)\s([A-Za-z]+)/, '$1') // -> returns 'Firsty'

'Firsty McLastname'.replace(/([A-Za-z]+)\s([A-Za-z]+)/, '$2, $1') // -> returns 'McLastname, Firsty'

'Firstipher Lasterman'.replace(/([A-Za-z]+)\s([A-Za-z]+)/, '$&') // -> returns 'Firstipher Lasterman'

#### Curly Braces
Curly braces are used to specify an exact amount of things to match. They are used after an expression: \na{2}\ will only match 'na' exactly twice.

'panama'.match(/na{2}/) // -> no match
'banana'.match(/na{2}/) // -> matches 'nana'
You can use these in conjunction with a comma to specify more than one amount. {2,} = two or more times, {2,4} = between two and four times.

'banana'.match(/a{2,4}/) // -> no match
'bananaa'.match(/a{2,4}/) // -> matches 'aa'
'bananaaa'.match(/a{2,4}/) // -> matches 'aaa'
'bananaaaa'.match(/a{2,4}/) // -> matches 'aaaa'
'bananaaaaaaaaaaa'.match(/a{2,4}/) // -> matches 'aaaa'

### Character Classes
Character classes distinguish kinds of characters such as, for example, distinguishing between letters and digits.

#### Types
[xyz],[a-c]	
A character class. Matches any one of the enclosed characters. You can specify a range of characters by using a hyphen, but if the hyphen appears as the first or last character enclosed in the square brackets, it is taken as a literal hyphen to be included in the character class as a normal character.

For example, [abcd] is the same as [a-d]. They match the "b" in "brisket", and the "c" in "chop".

For example, [abcd-] and [-abcd] match the "b" in "brisket", the "c" in "chop", and the "-" (hyphen) in "non-profit".

For example, [\w-] is the same as [A-Za-z0-9_-]. They both match the "b" in "brisket", the "c" in "chop", and the "n" in "non-profit".

[^xyz],[^a-c]
A negated or complemented character class. That is, it matches anything that is not enclosed in the brackets. You can specify a range of characters by using a hyphen, but if the hyphen appears as the first character after the ^ or the last character enclosed in the square brackets, it is taken as a literal hyphen to be included in the character class as a normal character. For example, [^abc] is the same as [^a-c]. They initially match "o" in "bacon" and "h" in "chop".

Note: The ^ character may also indicate the beginning of input.

.	
Has one of the following meanings:

Matches any single character except line terminators: \n, \r, \u2028 or \u2029. For example, /.y/ matches "my" and "ay", but not "yes", in "yes make my day", as there is no character before "y" in "yes".
Inside a character class, the dot loses its special meaning and matches a literal dot.
Note that the m multiline flag doesn't change the dot behavior. So to match a pattern across multiple lines, the character class [^] can be used — it will match any character including newlines.

The s "dotAll" flag allows the dot to also match line terminators.

\d	
Matches any digit (Arabic numeral). Equivalent to [0-9]. For example, /\d/ or /[0-9]/ matches "2" in "B2 is the suite number".

\D	
Matches any character that is not a digit (Arabic numeral). Equivalent to [^0-9]. For example, /\D/ or /[^0-9]/ matches "B" in "B2 is the suite number".

\w	
Matches any alphanumeric character from the basic Latin alphabet, including the underscore. Equivalent to [A-Za-z0-9_]. For example, /\w/ matches "a" in "apple", "5" in "$5.28", "3" in "3D" and "m" in "Émanuel".

\W	
Matches any character that is not a word character from the basic Latin alphabet. Equivalent to [^A-Za-z0-9_]. For example, /\W/ or /[^A-Za-z0-9_]/ matches "%" in "50%" and "É" in "Émanuel".

\s	
Matches a single white space character, including space, tab, form feed, line feed, and other Unicode spaces. Equivalent to [ \f\n\r\t\v\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]. For example, /\s\w*/ matches " bar" in "foo bar".

\S	
Matches a single character other than white space. Equivalent to [^ \f\n\r\t\v\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]. For example, /\S\w*/ matches "foo" in "foo bar".

\t	Matches a horizontal tab.
\r	Matches a carriage return.
\n	Matches a linefeed.
\v	Matches a vertical tab.
\f	Matches a form-feed.
[\b]	Matches a backspace. If you're looking for the word-boundary character (\b), see Assertions.
\0	Matches a NUL character. Do not follow this with another digit.
\cX	
Matches a control character using caret notation, where "X" is a letter from A–Z (corresponding to codepoints U+0001–U+001A). For example, /\cM\cJ/ matches "\r\n".

\xhh	Matches the character with the code hh (two hexadecimal digits).
\uhhhh	Matches a UTF-16 code-unit with the value hhhh (four hexadecimal digits).
\u{hhhh} or \u{hhhhh}	(Only when the u flag is set.) Matches the character with the Unicode value U+hhhh or U+hhhhh (hexadecimal digits).
\p{UnicodeProperty}, \P{UnicodeProperty}	Matches a character based on its Unicode character properties (to match just, for example, emoji characters, or Japanese katakana characters, or Chinese/Japanese Han/Kanji characters, etc.).
\	
Indicates that the following character should be treated specially, or "escaped". It behaves one of two ways.

For characters that are usually treated literally, indicates that the next character is special and not to be interpreted literally. For example, /b/ matches the character "b". By placing a backslash in front of "b", that is by using /\b/, the character becomes special to mean match a word boundary.
For characters that are usually treated specially, indicates that the next character is not special and should be interpreted literally. For example, "*" is a special character that means 0 or more occurrences of the preceding character should be matched; for example, /a*/ means match 0 or more "a"s. To match * literally, precede it with a backslash; for example, /a\*/ matches "a*".
Note: To match this character literally, escape it with itself. In other words to search for \ use /\\/.

x|y	
Disjunction: Matches either "x" or "y". Each component, separated by a pipe (|), is called an alternative. For example, /green|red/ matches "green" in "green apple" and "red" in "red apple".

### The OR Operator

### Flags

### Character Escapes

## Author

### Michael Yiu
Michael Yiu is a Mechanical Engineer and graduate of UC San Diego

Github Link: https://github.com/michaelyiu1