# How to use regular expressions

---

If you aim to find matching strings by a pattern, you can use [regular expressions](#link) (regex for short).
Regex also allows variously manipulating strings, such as finding a string and replacing it with another one
or splitting a string into parts according to a specific principle.

This article describes how to write regular expressions using the [Regexp][regex] class to:

* [extract dates from input strings](#extract-date-from-string);
* [validate email addresses](#validate-email-address);
* [format phone numbers](#format-phone-numbers).

> Refer to [Class Pattern][pattern-class] to learn about pattern syntax.

## Extract date from string

You can extract any data such as phone numbers, dates, or URLs from an input string
using the [`find()`][find] or [`findAll()`][find-all] functions.
`find()` finds the first match of a regular expression in the input string
when `findAll()` finds all occurrences.

For example, to find a date in format `mm-dd-yyyy` and extract it from a string:

1. Call the [`Regex`][constructor] constructor and pass the following argument into it: `"\\d{2}-\\d{2}-\\d{4}"`.
    * `\d` matches a digit from 0 to 9.
    * `{n}` matches the previous token (`\d`) exactly *n* times. Here, `\d{2}` matches two digits.
2. Call `find()` and pass a string as an argument.
    If a match is found, the function returns an instance of [`MatchResult`][match-result].
    Otherwise, it returns `null`.

    > Note that the input string may not contain matches, so don’t forget to use the `!!` operator.

3. Extract the result of regular expression from a `value` property of `MatchResult`.

```kotlin
fun main() {
    // Create regex for dates
    val dateRegex = Regex("\\d{2}-\\d{2}-\\d{4}")
    val date = dateRegex.find("Google announced on 05-07-2019 that Kotlin is now its preferred language for Android app developers.")!!
    
    println(date.value) // => "05-07-2019"
}
```

> If you need to find all matches of a regular expression, use [`findAll()`][find-all] instead.

## Validate email address

Validating email addresses is one of the use cases for regular expressions.
For example, to check if an email conforms to a template such as `name.surname@jetbrains.com`:

1. Call the [`Regex`][constructor] constructor and pass the following argument into it: `"^[a-z]+\\.[a-z]+@jetbrains\\.com$"`.

    * `^` and `$` indicates the beginning and the end of a line. It means that only whole string matches are allowed.
    * `[a-z]+` matches any characters between a-z one or more times. This part stands for a user’s name.
    * `\\.` matches only one dot between name and surname.

        > Don’t forget to escape special characters such as dot.

    * `[a-z]+` matches any characters between a-z. This part stands for a user’s surname.
    * `@jetbrains\\.com` matches the `@` character and the `jetbrains.com` domain name.

2. Call the [`matches()`][matches] function to check whether the regular expression matches the entire input.
    The function returns a boolean result.

```kotlin
fun main() {
    // Create regex for emails
    val emailRegex = Regex("^[a-z]+\\.[a-z]+@jetbrains\\.com$")

    // Check if an email matches the pattern
    println(emailRegex.matches("james.smith@jetbrains.com"))   // => "true"
    println(emailRegex.matches("jamessmith@jetbrains.com"))    // => "false"
    println(emailRegex.matches("james.smith@@jetbrains.com"))  // => "false"
}
```

## Format phone numbers

You can use regular expressions to format input phone numbers.
For example, to separate the country and area codes from a phone number:

1. Call the [`Regex`][constructor] constructor and pass the following argument into it: `"(\\d)(\\d{3})(\\d{3})(\\d{4})"`.

    * `\d` matches a digit from 0 to 9.
    * `{n}` matches the previous token (`\d`) exactly *n* times. Here, `\d{3}` matches three digits.
    * `(...)` captures everything enclosed. Here, the template consists of four capturing groups.

2. Call the [`replace()`][replace] function.
3. As a `replacement` parameter, specify a string that contains references to the capturing groups.
4. The function returns the replacing result.

```kotlin
fun main() {    
    // Create a regex for phone numbers
    val phoneNumberRegex = Regex("(\\d)(\\d{3})(\\d{3})(\\d{4})")
    val phoneNumberExample = "12345678901"
    
    println(phoneNumberExample.replace(phoneNumberRegex, "+$1 ($2) $3-$4")) // => "+1 (234) 567-8901"
}
```

[constructor]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/-regex/#constructors
[find]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/-regex/find.html
[find-all]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/-regex/find-all.html
[matches]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/-regex/matches.html
[match-result]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/-match-result/
[pattern-class]: https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html
[regex]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/-regex/
[replace]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin.text/-regex/replace.html
