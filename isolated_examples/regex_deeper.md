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

`$  # Count the number of error messages in logfile.txt `<br/>
`$` ${{\color{BrickRed}\texttt{ \quad grep \quad }}}$ 
<kbd>-</kbd> ${{\color{BrickRed}\texttt{i \quad}}}$ 
<kbd>'</kbd> ${{\color{DarkOrange}\small{\texttt{error}}}}$ <kbd>'</kbd> 
${{\color{BrickRed}\small{\texttt{ logfile.txt }}}}$ 
` | wc -l`<br/>

`$  # Does something quite complicated; cf. man grep `<br/>
`$` ${{\color{BrickRed}\texttt{ \quad grep \quad }}}$ 
<kbd>-</kbd> ${{\color{BrickRed}\texttt{n \quad}}}$
<kbd>--</kbd> ${{\color{DarkOrange}\texttt{ \quad }}}$
<kbd>'</kbd> ${{\color{DarkOrange}\small{\texttt{f}}}}$ <kbd>.*</kbd> <kbd>\\.</kbd> ${{\color{DarkOrange}\small{\texttt{c}}}}$ <kbd>$</kbd> <kbd>'</kbd>
${{\color{BrickRed}\texttt{ \*g\*.h \quad}}}$
<kbd>&gt</kbd>
${{\color{BrickRed}\texttt{ \*g\*.h \quad}}}$


${{\color{BrickRed}\texttt{ \quad }}}$
<kbd>'</kbd> 
${{\color{DarkOrange}\small{\texttt{f}}}}$ 
<kbd>.*\.</kbd>
${{\color{DarkOrange}\small{\texttt{c}}}}$
<kbd>'</kbd> 
${{\color{BrickRed}\small{\texttt{ logfile.txt }}}}$ 
` | wc -l`<br/>


<br/>
