For all of these cases, MathJax&mdash;or whatever's rendering the LaTeX&mdash;can struggle with non-alphanumeric symbols. 
In these cases, rather than digging too deep into LaTeX stuff, keyboard styling will be used for <kbd>the symbols</kbd>, 
e.g. <kbd>'</kbd> or <kbd>-</kbd> or <kbd>_</kbd>. Note that this allowed for somewhat-feasible copy/paste 
(though you'll need to take out some spaces), where other solutions didn't.

<code>I'll put my </code> 
${{\color{Cerulean}\small{\texttt{ \quad awk \quad code \quad }}}}$ 
<code> in </code> 
${{\color{Cerulean}\small{\texttt{ \quad blue \quad }}}}$

<code>I'll put my (not-specifically-regex) </code> 
${{\color{BrickRed}\small{\texttt{ \quad grep \quad code \quad }}}}$ 
<code> in </code> 
${{\color{BrickRed}\small{\texttt{ \quad red \quad }}}}$

<code>I'll put my (not-specifically-regex) </code> 
${{\color{ForestGreen}\small{\texttt{ \quad sed \quad code \quad }}}}$ 
<code> in </code> 
${{\color{ForestGreen}\small{\texttt{ \quad green \quad }}}}$

<code>I'll put my </code> 
${{\color{DarkOrange}\small{\texttt{ \quad code \quad for \quad regexes \quad }}}}$ 
<code> in </code> 
${{\color{DarkOrange}\small{\texttt{ \quad orange \quad }}}}$

Here are a few examples whose text might be familiar&mdash;they come from `man` pages 
or a `SOME-COMMAND` `--help` output or as part of the usage returned from an error. 
Often, they'll be slightly modified. Also, you might need to search `man awk`,
`man gawk`, `man mawk`, or `awk --help`, `gawk --help`, `mawk --help` before you find the example

`  $` `  # Count the number of error messages in logfile.txt `<br/>
`  $` ${{\color{BrickRed}\texttt{ \quad grep \quad }}}$ 
<kbd>-</kbd> ${{\color{BrickRed}\texttt{i \quad}}}$ 
<kbd>'</kbd> ${{\color{DarkOrange}\small{\texttt{error}}}}$ <kbd>'</kbd> 
${{\color{BrickRed}\small{\texttt{ logfile.txt }}}}$ 
` | wc -l`<br/>

`  $` `  # Does something quite complicated; cf. man grep `<br/>
`  $` ${{\color{BrickRed}\texttt{ \quad grep \quad }}}$ 
<kbd>-</kbd> ${{\color{BrickRed}\texttt{n \quad}}}$
<kbd>--</kbd> ${{\color{DarkOrange}\texttt{ \quad }}}$
<kbd>'</kbd> ${{\color{DarkOrange}\small{\texttt{f}}}}$ <kbd>.*</kbd> <kbd>\\.</kbd> ${{\color{DarkOrange}\small{\texttt{c}}}}$ <kbd>$</kbd> <kbd>'</kbd>
${{\color{BrickRed}\texttt{ \*g\*.h \quad}}}$
<kbd>&gt;</kbd>
${{\color{BrickRed}\texttt{ \quad /dev/null }}}$

<br/>
<hr/>
<br/>

Stuff from my other efforts:

`  $` `find . -type f -iname "*.png" | ` 
${{\color{BrickRed}\texttt{ \quad grep \quad }}}$
<kbd>"</kbd> <kbd>_</kbd> ${{\color{DarkOrange}\small{\texttt{p0}}}}$ <kbd>-</kbd> <kbd>"</kbd>
` | wc -l  #  Unique image files`

`1025`<br/>

`  $ find . -type f -iname "*.png" |`
${{\color{BrickRed}\texttt{ \quad grep \quad }}}$ 
<kbd>-</kbd> ${{\color{BrickRed}\texttt{v \quad}}}$
<kbd>"</kbd> <kbd>_</kbd> ${{\color{DarkOrange}\small{\texttt{p0}}}}$ <kbd>-</kbd> <kbd>"</kbd>
` | wc -l  #  Number of duplicates`

`2324`<br/>

`  $ #  We can double-check`<br/>
`  $ find . -type f -iname "*.png" |` 
${{\color{BrickRed}\texttt{ \quad grep \quad }}}$
<kbd>"</kbd> <kbd>_</kbd> ${{\color{DarkOrange}\small{\texttt{p1}}}}$ <kbd>-</kbd> <kbd>"</kbd>
` | wc -l  #  Duplicated the first time`

`1025`<br/>

`  $ find . -type f -iname "*.png" |` 
${{\color{BrickRed}\texttt{ \quad grep \quad }}}$
<kbd>"</kbd> <kbd>_</kbd> ${{\color{DarkOrange}\small{\texttt{p2}}}}$ <kbd>-</kbd> <kbd>"</kbd>
` | wc -l  #  Duplicated the second time`

`1025`<br/>

`  $ find . -type f -iname "*.png" |` 
${{\color{BrickRed}\texttt{ \quad grep \quad }}}$
<kbd>"</kbd> <kbd>_</kbd> ${{\color{DarkOrange}\small{\texttt{p3}}}}$ <kbd>-</kbd> <kbd>"</kbd>
` | wc -l  #  Duplicated the third time`

`274`<br/>

`  $ find . -type f -iname "*.png" |` 
${{\color{BrickRed}\texttt{ \quad grep \quad }}}$
<kbd>"</kbd> <kbd>_</kbd> ${{\color{DarkOrange}\small{\texttt{p4}}}}$ <kbd>-</kbd> <kbd>"</kbd>
` | wc -l  #  Should have 0 duplctd 4th time`

`0`<br/>

```bash
$ echo "1025+1025+274" | bc
2324
```

<br/>
<hr/>
<br/>

```bash
$ find . -type f -iname "*.png" |
    xargs -I{} -0 bash -c '
      orig={};
```
`        new_fname=$(echo "${orig}" |` ${{\color{Cerulean}\small{\texttt{ \quad awk \quad }}}}$ <kbd>-</kbd> ${{\color{Cerulean}\small{\texttt{ F \quad }}}}$ <kbd>'"'"'</kbd> <kbd>.</kbd> <kbd>'"'"'</kbd> ${{\color{Cerulean}\small{\texttt{ \quad \quad }}}}$ <kbd>'"'"'</kbd> <kbd>{</kbd> ${{\color{Cerulean}\small{\texttt{ print }}}}$ <kbd>$</kbd> ${{\color{Cerulean}\small{\texttt{ 1 \quad}}}}$ <kbd>"</kbd> <kbd>_</kbd> ${{\color{Cerulean}\small{\texttt{ 001 }}}}$ <kbd>"</kbd> ${{\color{Cerulean}\small{\texttt{ \quad}}}}$ <kbd>.</kbd> <kbd>$</kbd> ${{\color{Cerulean}\small{\texttt{ 2 \quad}}}}$ <kbd>}</kbd> <kbd>'"'"'</kbd> `);`
```bash
      echo "  Renaming ${orig} to ${new_fname}";
      echo "       ...";
      mv "${orig}" "${new_fname}" && echo "           ... success" || echo "           ... FAILURE";
      echo
    ' 2>&1 | tee outfile001.out
```

`  $` ${{\color{BrickRed}\texttt{ \quad grep \quad }}}$ 
<kbd>-</kbd> ${{\color{BrickRed}\texttt{i \quad}}}$
<kbd>--</kbd> ${{\color{DarkOrange}\texttt{failure \quad}}}$
${{\color{BrickRed}\small{\texttt{ outfile001.out }}}}$
` | wc -l  # n_failures?`

<strong>`  grep: unknown option -- failure`</strong>

```bash
Usage: grep [OPTION]... PATTERNS [FILE]...
Try 'grep --help' for more information.
0
```

### My favorite error; basically, "Failure is not an option" X D

Now, for what I should really check for

`  $` ${{\color{BrickRed}\texttt{ \quad grep \quad }}}$ 
<kbd>-</kbd> ${{\color{BrickRed}\texttt{i \quad}}}$
<kbd>"</kbd> ${{\color{DarkOrange}\texttt{failure}}}$ <kbd>"</kbd>
${{\color{BrickRed}\small{\texttt{ outfile001.out }}}}$
` | wc -l  # n_failures?`

```bash
0

$ #  Great! No failures
```
