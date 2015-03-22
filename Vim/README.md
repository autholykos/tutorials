# Vim as a Node/JS IDE

Vim is my choice of IDE. I like it because you can do things that are impossible in any other text editor. Since with great powers come great responsibilities, VIM is a tad bit more complicated than other edtitors and requires some setup by itself.

This won't be an extended tutorial about VIM as much as a tutorial about using VIM as an IDE to program in Node.

Vim comes with great **plugins** that help developers achieve an incredible number of tasks.

## Vundle

[Vundle](https://github.com/gmarik/Vundle.vim) stands for *Vim Bundle* and its aim is to makes it very simple to manage Vim plugins.

Quoting `Vundle` directly,

> Installation requires [Git] and triggers [`git clone`] for each configured repository to `~/.vim/bundle/` by default. Curl is required for search.

## NERDTree

A very cool functionality of NERDTree is the possibility to find text in a path.

```vim
:set path=/path/to/proj/**

:find keyword
```

## Regexp

Vim regexp support is truly phenomenal. 
The comprehensive documentation for it can be found at the (vimregexp)[http://vimregex.com/].

Following is the form of a regexp substitution

    `*range* s[ubstitute]/pattern\(group\)/string\matched_group/*additional cmds*`

where the *additional cmds* are

Cmd | meaning
:---|--------
g | Replace all occurrences in the line instead of only the first
c | Confirm each substitution
i | ignore case for pattern
I | do not ignore case for patter

### Possible ranges

Specifier | meaning
----------|--------
. | current line
% | all lines
$ | last line
't | position of mark "t"
/pattern/ | next line matching pattern
\/ | next line where the last searched pattern matches
\? | previous line where the last searched pattern matches
\& | next line where the last substitution pattern matched

### Table of escaped characters

Special Char | Matching | Opposite 
:------------|:--------:|:--------
`.` | everything except new lines |  
`\s` | whitespaces| `\S`
`\d` | digits | `\D`
`\w` | word char | `\W`
`\l` | lowercase | `\L`
`\u` | uppercase | `\U`
`\x` | hexadecimal | `\X`
`\o` | octal | `\O`
`\h` | head of word chars (a..zA..Z and \_) | `\H`

For example, if I want to change a numbered list with `<h2>` headlines, you can use the following regexp

    ```vim
    :%s/^\d\.\s\+/## /gi
    "substitutes a digit, a point and an arbitrary number of spaces greated than 1 at the beginning of the line with '## '
    ```

## Table of quantifiers

Quantifier | meaning | greedy?
:----------|:-------:|:------:
* | 0 or more of preceding char | V
\+ | 1 or more of preceding char | V
\= | 0 or 1 of preceding char | V
\{n} | exactly *n* times of preceding char | V
\{n,m} | between *n* and *m* of preceding char | V
\{,m} | at most *m* of preceding char | V
\{n,} | at least *n* of preceding char | V
\{-} | 0 or more of preceding atom as few as possible | X
\{-n,m} | 1 or more of preceding char | X

## Table of grouping chars

Specified | meaning
----------|:-------:
& | The whole matched pattern
\0 | The whole matched pattern
\1 | matched pattern in the first group of \(\)
\2 | matched pattern in the second pair of \(\)
~ | previous substitute string
\L | following chars are made lowercase
\l | next char is made lowercase
\U | following chars are made uppercase
\u | next char is made uppercase
\E | end of \U and \L
\r | split line in two at this point

for example:

    ```vim
    s:\(\w\+\)\(\s\+\)\(\w\+\):\3\2\1:
    " Substitutes the first two words of a line
    ```
