#Regular Expressions
-

## What we'll cover

<p class="fragment fade-up">Regex basics</p>
<p class="fragment fade-up">Pattern Matching</p>


-
### Resources

- [Regexr](https://regexr.com/3m8em)
- [Regular-expressions.info](http://www.regular-expressions.info/)
- [Regex Crossword](https://regexcrossword.com/) - Regex-based challenges
- [Duck Duck Go Regex Cheat Sheet](https://duckduckgo.com/?q=regex+cheat+sheet&ia=cheatsheet)
- [Pattern class documentation](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html) - includes extensive regex explanations.

-
-
# Regex basics

-
### Basic symbols

- `a`, `b`, `c` - match "a", "b", "c" respectively
	* Works with all numbers and letters
	* Case sensitive e.g. `a` does not match "A" and `B` does not match "b"
- `ab` matches "ab"
	* Works with any string
	* invisible characters like spaces are also match 
	* `a b` does not match "ab"

-

### Dot matching

`.` - matches any one character

* `.` matches "a" and "A" and "b" and "c" etc.
* `a.c` matches "abc" and "acc" and "a c" etc.

-

### Repeated matches

- `+` - match 1 or more occurrences of the last symbol
	* `a+` matches "a" and "aa" and "aaa" etc.
	* `ab+` matches "ab" and "abb" but not "a" or "b"
- `*` - match 0 or more occurrences of the last symbol
	* `ab*` matches "ab" and "abb" and "abbb" etc.
	* `ab*` will also match "a"

-

### Repeated matches (continued)

- `{n}`, `{n,m}` - Repeat previous match *n* times or *n* to *m* times
	* `a{1}` matches "a"
	* `a{2}` does not match "a" but does match "aa"
	* `a{1,3}` matches all of "a" and "aa" and "aaa"

-

### Advanced Matching

- `()`- group characters together 
	* `(abc){3}` matches "abcabcabc" and does not match "abccc"
- `|` - alternation, matches either the pattern on the left or the right
	* `a|b` will match both "a" and "b" but not "ab"
	* note `a|b` will still find both "a" and "b" in "ab" but it will be as two matches and not one
- `?` - match 0 or 1 occurrences of the last symbol
	* `ab?` will match "a" and "ab"

-

### Boundaries

- `^` - Beginning of line
	* `^a` will find the "a" in "ab" but not in "ba"
- `$` - End of line
	* `a$` will find the "a" in "ba" but not in "ab"
- `\b` - Word boundary
	* `\bman` will match the "man" in "The man" but not in "The human"

-
-

### Character classes

- Match any single character in the character class
- groups of characters within [] brackets 
	* `[abc]` matches "a", "b" or "c"
- hyphen denotes a range (eg: `[a-c]` == `[abc]`
- Negate a character class with `^` 
	* `[^13579]` matches any character other than odd numbers
- Note: Many special characters behave differently inside of character classes than they do outside of them.

-
### Predefined character classes

Shortcuts for commonly used character classes

- `\s` - any whitespace character (`\\s` in Java Strings)
- `\S` - non-whitespace characters aka `[^\s]` (`\\S` in Java Strings)
- `\d` - any numeric digit aka `[0-9]` (`\\d` in Java)
- `\D` - any non-digit aka `[^0-9]` (`\\D` in Java)
- `\w` - a word character `[a-zA-Z_0-9]`
- `\W` - a non-word character aka `[^\w]`


-
-

## Using Regex in Java

-
### Quirks

- Java Strings are converted to regex patterns -- this means backslashes and escape sequences are parsed and interpreted as what they represent in a String
- `\n` and `\t` (and some others) become the character they represent; `\\` becomes `\` and `\\\\` becomes `\\` 
- As a result, Java regex escape sequences (such as `\d`, `\.`, `\\`) must be double-escaped (eg: `\\d`, `\\.`, `\\\\`)

-
### Methods and Classes

- Used in String methods like `replaceAll`, `replaceFirst`, `split`, and `matches`
- `Pattern` objects represent compiled regular expressions (using `Pattern thePattern = Pattern.compile(regexStr)`
- `Matcher` objects provide information about matching a regex on to an input, created with `Matcher m = thePattern.matcher(inputStr);`

-
### Examples

```
public boolean batman(String str){
  return str.matches("(na){16}");
}
```
```
public boolean manOfSteel(String str){
  return str.matches("(Clark Kent)|(Superman)|(Kal-El)");
}
```
```
public boolean time(String str){
  return str.matches("w[io]bbly");
}
```

-
### Backtracking

- Aforementioned Regex quantifiers are "greedy" - They will match the most characters possible to still allow the pattern to match
- Greedy quantifiers start at their longest and backtrack one match at a time until the whole pattern matches.
- Beware of [catastrophic backtracking](http://www.regular-expressions.info/catastrophic.html)
- `.*` is very, **very** suspicious.

-
### Lazy quantifiers

AKA Reluctant quantifiers

- consume smallest possible sequence to achieve a match.
- Same syntax as greedy quantifiers, plus a `?` at the end
- eg: `abc*?`, `[0-9]+?3`

-

<img src="https://img.buzzfeed.com/buzzfeed-static/static/2017-09/11/15/asset/buzzfeed-prod-fastlane-01/sub-buzz-16785-1505158685-6.jpg" alt="cute bats">
