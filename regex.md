## Regex

Common uses:

- Validations (Phone numbers, Emails, Domains)
- Searching words in a sentence
- Searching unwanted characters
- Extracting Sections / Replacing / Cleaning / Formatting

Regular Expression

The pattern starts with `/` and ends `/`, for example `/car/` match with `car` but no with `kar`.

### Or Operator

```
/todd|dan/
```
The `|` operator allows matches with two different patterns, the expression above matches with `dan` or `todd`, but not with `mike`.
### Plus Operator

e.g
```
/ar+/
```
The `+` operator (Quantifier) matches `r` 1 or more times until no longer matched. So this expression matches with `ar` ,`arr` or `arrrr`

### Character Set

-  [a-z]
-  [A-Z]
-  [0-9]

/[a-zA-z]/ === /a-z/i

\w === [a-zA-z0-9]

white spaces  `\s`

### match especial characters
  \. matches literal .
  . macthes any characters except new line
  \+ matches literal +

  ? optional

  $ stop looking at the end of the subject
  ^ start looking at the beginning of the subject

  \b Boundaries

```
/\b(ok(ay)?|sure)\b/i
```

posssible subjects:

- Ahooy, `Okay`!.
- `ok`, i will do it.
