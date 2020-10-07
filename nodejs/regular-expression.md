# Using the Test Method
Regular expressions are used in programming languages to match parts of strings. You create patterns to help you do that matching.

If you want to find the word "the" in the string "The dog chased the cat", you could use the following regular expression: `/the/`. 

Javscript has multiple way to use regexes. One way to test as regex is using the `.test()` method. The `.test()` method takes the regex, applies it to a string(which is placed inside the parentheses), and return `true` or `false` if you pattern finds something or not.

```javascript
let testStr = "freeCodeCamp";
let testRegex = /Code/;
testRegex.test(testStr); //Return true
```
## Match Literal Strings
The regex searched for a literal match of the string "Hello". Here's another example searching for literal match of the string `"Kevin"`:

```javascript
let testStr = "Hello, my name is Kevin.";
let testRegex = /Kevin/;
testRegex.test(testStr); //Return true
```

Any other forms of `"Kevin" will not match. For example, the `/Kelvin/` will not match `"kelvin"` or `"KELVIN"`.

Using regexes like `/coding/` you can look the pattern `"coding"` in another string.

This is powerful to search single strings, but it's limited to only one pattern. You can search for multiple patterns using the `alternation` or `OR` operator : `|`.

This operator matches pattern either before or after it. For example, if you wanted to match "yes" or "no", the regex you want is `/yes|no/`.

You can also search for more than just two patterns. You can do this by adding more patterns with more `OR` operators separating them, like `/yes|no|maybe/`.

Up until now, yo've looked at regexes to do literal matches of strings. But sometimes, you might want to also match case differences.

Case(or sometimes letter case) is the difference between uppercase letter and lowercase letters. Examples of uppercase are "A", "B", and "C". Example of lowecase are "a", "b", and "c".

You can match both cases using what is called a flag. There are other flags but here you'll focus on the flag that igonore case - that `i` flag. You can use it by appending it to the regex. An example of using this flag is `/ignorecase/i`. This regex can match the strings `"ignonrecase"`, `"igNoreCase"`, and `"IgnoreCase"`.

## Extract Matches
So far, you have only been checking if a pattern exists or not within a string. You can also extract the actual matches you found with the `.match()` method.

To use the `.match()` method apply the method on a string and pass in the regex inside the parentheses.

```javascript
"Hello World".match(/Hello/); //Return ['Hello']
```

So far, you have only been able to extract or search a pattern once.

```javascript
et testStr = "Repeat, Repeat, Repeat";
let ourRegex = /Repeat/;
testStr.match(ourRegex); //Return ["Repeat"]
```

To search or extract a pattern more than once, you can use the `g` flag.

```javascript
let repeatRegex = /Repeat/g;
testStr.match(repeatRegex); // Returns ["Repeat", "Repeat", "Repeat"]
```
## Match Anything with Wildcard Period.
Sometines you won't (or don't need to) know the exact characters in your pattern. Thinking of all words that match, say a misspelling would take a long time. Luckily, you can save time using the wildcard character:`.`

The wildcard character `.` will match any one character. The wildcard is also called `dot` and `period`. You can use the wildcard character just like any other character in the regex. For example, if you wanted to match "hug", "huh", "hut", and "hum", you can use the regex `/hu./` to match all four words.

## Match Single Character with multiple possibilities
You can search for a literal pattern with some flexibility with a character classes. Character classes allow you to define a group of characters you wish to march by placing them inside square (`[` and `]`) brackets.

For example, you want to match "bag", "big" and "bug" but not "bog". You can create the regex `/b[aiu]g/` to do this. The `[aiu] is the character that will only match the characters "a", "i" and "u".

You saw how you can use character sets to specify a group of characters to match, but that's a lot of typing when you need to match a large range of characters. Fortunately, there is a built-in feature that makes this short and simple.

Inside a character set, you can define a range of characters to match using a hypen character: `-`.

For example, to match lowercase letters `a` through `e` you would use `[a-e]`.

Using the hypen (`-`) to match a range of characters is not limited to letters. It also works to match a range of numbers. For example, `/[0-5]/` matches any number between 0 and 5, including the  0 and 5.

So far, you have created a set of characters that you want to match, but you could also create a set of characters that you do not want to match. These types of characters sets are called `negated character sets`.

To create a negated character set, you place a caret character (`^`) after the opening bracet and before the characters you do not want to match.
For example - `/[^aeiou]/gi` matches all characters that are not a vowel. Note that characters like `.`, `!`, `[`, `@`, `/` and whitespace are matched - the negated vowels characters set only excludes the vowel characters.

Sometimes, you need to match character (or group of characters) that appears one or more times in a row. This means it occur at least once, and may be repeated.

You can use the `+` charcter to check if that is the case. Remember, the character or pattern has to be present consecutively. That is, the character has to repeat one after the other.

For example, `/a+/g` would find one match in "abc" and return ["a"] . Beacause of the `+`, it would also find a single match in "aabc" and return ["aa"]

If it were instead of checking the string "abab", it would find two matches and return ["a", "a"] because the `a` characters are not in a row - there is a `b` between them. Finally, since there is no "a" in the string "bcd", it wouldn't find a match.

There;s also an option that matches characters that occur zero or more times. The character to do this is the asterisk or star: `*`.

## Finding Characters with Lazy Matching
In regular expressions, a greedy match finds the longest possible part of a string that fits the regex pattern and returns it as match. The alternative is called a lazy match, which finds the smallest possible part of the string that satisfies the regex pattern.

You can apply the regex `/t[a-z]*i/` to the string "titanic". This regex is basically pattern that starts with `t` , ends with `i` and has some letter in between.

Regular expression are by default greedy, so the match would return ["titani"]. It finds the largest sub-string possible to fit the pattern.

However, you can use the `?` character to change it to lazy matching. "titanic" matched against the adjusted regex of `/t[a-z]*?i/` returns ["ti"]

## Match Begining String patterns
We used caret character (^) inside a character set to create a negated character set in the form `[^thingThatWillNotBeMatched]`. Outside of a character set, the caret is used to search for patterns at the begining of strings.

## Match Ending String patterns
There is also way to search for patterns at the end of strings. You can search the end of strings using the dollor sign character `$` at the end of the regex.

## Match All letters and numbers
Using character classes, you were able to search for all letters of the alphabet with `[a-z]`. This kind of characters class is common enough that there is a shortcut for it, although it include a few extra characters as well.

The closest character class in Javascript to match the alphabet is `\w`. This shortcut is equal to `[A-Za-z0-9_]`. This character class matches upper and lowercase letters plus numbers . Note, this character class includes the underscore character(_).

You can search the opposite of the `\w` with `\W`. Note, the opposite patterns uses a capital letter. This shortcut is the same as `[^A-Za-z0-9_]`.

The shortcut to look for digit characters is `\d`, with a lowercase `d`. This is equal to the character class `[0-9]`, which looks for a single character of any number between zero and nine.

The shortcut to look for non-digit characters is `\D`.

## Match Whitespace
You can match whitespace or spaces between letters

You can search for whitespace using `\s`, which is a lowercase `s`. This pattern not only matches whitespace, but also carriage return tab, form feed, and new line characters. You can think of it as similar to the character class `[\r\t\f\n\v]`.

Search for non-whitepspae using `\S`, which is uppercase `s`. This pattern will not match whitespace, carriage return tab, form feed and new line characters. You can think of it being similar to the character class `[^\r\t\f\n\v]`.

## Specify Upper and Lower Number of matches
Recall that you use the plus sign `+` to look for one or more character and the asterik `*` to look zero or more characters. These are convenient but sometimes you want to match a certain range of patterns.

You can specify the lower and upper number of patterns with quantity specifiers. Quantity specifier are used with curly brackets (`{` ad `}`). You put two numbers between the curly brackets - for the lower and upper number of patterns

```javascript
let A4 = "aaaah";
let A2 = "aah";
let multipleA = /a{3,5}h/;
multipleA.test(A4); // Returns true
multipleA.test(A2); // Returns false
```
YOu can specify the lower and upper number of patterns with quantity specifiers using curly brackets. Sometimes you only want to specify the lower number of patterns with no upper limit.

To only specify the lower number of patterns, keep the first number followed by a comma.

For example, to match only the string `hah` with the letter `a` appearing at least `3` times, your regex would be `/ha{3,}h/`.

To specify a certain number of patterns, just have that one number between the curly brackets. 

For example, to match only the word `"hah"` with the letter `a` `3` times, your regex would be `/ha{3}h/`.

## Check for All or None
Sometimes the pattern you want to search for may have parts of its that may or may not exist. However, it may be important to check for them nonetheless.

You can specify the possible existence of an element with a question mark `?`. This check for zero or one of the preceding element. You can think of this symbol as saying the previous element is optional.

For example, there are slight differences in American and British English and you can use the question mark to match both spellings.

```javascript
let american = "color";
let british = "colour";
let rainbowRegex= /colou?r/;
rainbowRegex.test(american); // Returns true
rainbowRegex.test(british); // Returns true
```
## Postive and Negative lookahead
Lookaheads are patterns that tell Javascript to look-ahead in your string to check for patterns further along. This can be useful when you want to search for multiple patterns over the same string.

There are two kinds of lookaheads: postive and negative lookhead.

A postive lookhead will look to make sure the element in the search pattern is there, but won't actually match it. A postive lookhead is used as `(?=...)` where `...` is the required part that is not matched.

On the other hand, a negative lookahead will look to make sure the element in the search pattern is not there. A negative lookahead is used as `(?!...)` where `...` is the pattern that you do not want to be there. The reset of the pattern is returned if the negative lookahead part is not present.

Lookahead are bit confusing but some examples will help

```javascript
let quit = "qu";
let noquit = "qt";
let quRegex= /q(?=u)/;
let qRegex = /q(?!u)/;
quit.match(quRegex); // Returns ["q"]
noquit.match(qRegex); // Returns ["q"]
```

A more pratical use of lookaheads is to check two or more patterns in one string. Here is a (naively) simple password checker that looks for between 3 and 6 characters and at least one number.

```javascript
et password = "abc123";
let checkPass = /(?=\w{3,6})(?=\D*\d)/;
checkPass.test(password); // Returns true
```

## Check for Mixed Grouping og characters
Sometimes we want to check for groups of characters using a Regular Expression and to achieve that we use parentheses `()`.

If you want to find either `Penguin` or `Pumpkin` in a string, you can use the following Regular expression: `/P(engu)|(umpk)in/g`.

Then check whether the desiered string groups are in the test string by using the `test()` method.

```javascript
let testStr = "Pumpkin";
let testRegex = /P(engu|umpk)in/;
testRegex.test(testStr);
```

## Reuse Patterns Using Capture Groups
Some patterns you search for will occur multiple times in a string. It is wasteful to manually repeat that regex. There is a better way to specify when you have multiple repeat substrings in your string.

You can search for repeat substrings using `capture groups`. Parentheses, `(` and `)`, are used to find repeat substrings. You put the regex of the pattern that will repeat in between the parentheses. 

To specify where the repeat string will appear, you use a backslash (`\`) and then a number. This number starts at 1 and increase with each additional capture group you use. An example would be `\1` to match the first group.

The example below matches any word that occurs twice separated by a space:

```javascript
let repeatStr = "regex regex";
let repeatRegex = /(\w+)\s\1/;
repeatRegex.test(repeatStr); // Returns true
repeatStr.match(repeatRegex); // Returns ["regex regex", "regex"]
```

## Capture groups to Search and replace
Searching is useful. However, you can make searching even more powerful when it also changes(or replaces) the text you match.

You can search and replace text in a string using `.replace()` on a string. The inputs for `.replace()` is first the regex pattern you want to search for. These second parameter is the string to replace the match or a function to do something.

```javascript
let wrongText = "The sky is silver.";
let silverRegex = /silver/;
wrongText.replace(silverRegex, "blue");
// Returns "The sky is blue."
```

