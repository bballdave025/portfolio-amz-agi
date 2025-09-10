# Some various Regular Expression usage and other scripting in `bash` 

## Background (a skippable section)

These examples are part of the dataset preparation for my classification of different types of
of Reused Manuscript Fragments in Bindings (RMFB). 

[Skip to the regex part](https://github.com/bballdave025/portfolio-amz-agi/edit/main/isolated_examples/regex_simple.md#the-regex-part)

<sub>
Description `README`s 
<a href="https://github.com/bballdave025/fhtw-paper-code-prep/tree/main"
   target="_blank">
  1
</a> 
& 
<a href="https://github.com/bballdave025/manuscript-waste-reuse-finder/main"
   target="_blank">
  2
</a>,
more detail in 
<a href="https://colab.research.google.com/github/bballdave025/rib-wrist-in-bin-din/blob/main/Paper_Code_Prep_01.ipynb"
   target="_blank">
  this Colab notebook]
</a>
& 
<a href="https://docs.google.com/document/d/1JAIL4PFmIm3_gScfscTj88yXKjorG7NIcXnTlT2IcSI/edit?usp=sharing
   target="_blank">
  this extended abstract
</a>
and literature review catch-all, hosted on Google Drive. None of that is necessary for understanding 
this part of my portfolio.
</sub>

<br/><br/>

The examples I show here came specifically from working of the conversion of
public-domain/open-access PDF documents containing images of old books, many of which are 
manuscripts on parchment (animal skin treated for writing) or paper contemporary to the 
parchment. There is also printed material. This is part of a process document (step-by-step
reminder) for going through the process quickly and preparing more and more automation. I 
also show some of the patterns I often use in `bash` scripting, including the use of `find` 
and several file-handling operations. The specific process (not all of which will be shown)
was changing filenames from the output-style of a previously-coded image extractor to the
style I wanted for the RMFB project.

Here, I have used some markdown to highlight certain
commands and processes that were originally part of my 
[terminal logfile](https://github.com/bballdave025/fhtw-paper-code-prep/blob/main/dataset_preparation_examples/steps_walkthrough_pdf_through_jpeg.log) 
for the day.

# The regex part

There were plans to have some [seriously color-coded commands and regexes&mdash;[example 1](https://github.com/bballdave025/portfolio-amz-agi/blob/main/isolated_examples/regex_deeper.md) & [example 2](https://github.com/bballdave025/fhtw-paper-code-prep/blob/main/dataset_preparation_examples/steps_rename_utrecht.md)&mdash;but I decided becoming an expert in LaTeX typography wasn't my first priority.

Those commands which I especially want to highlight will be in <kbd>keyboard style</kbd> instead of <code>code style</code>. These include regex usage and general `bash` usage`. I'll include a few examples with some combined color-coding and keyboard style.

## Some more-simple regexes&mdash;sometimes lacking context&mdash;with color coding

Many of the same will be seen in-context in the next section

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
`        new_fname=$(echo "${orig}" |` ${{\color{Cerulean}\small{\texttt{ \quad awk \quad }}}}$ <kbd>-</kbd> ${{\color{Cerulean}\small{\texttt{ F \quad }}}}$ <kbd>'"'"'</kbd> <kbd>.</kbd> <kbd>'"'"'</kbd> ${{\color{Cerulean}\small{\texttt{ \quad \quad }}}}$ <kbd>'"'"'</kbd> <kbd>{</kbd> ${{\color{Cerulean}\small{\texttt{ print }}}}$ <kbd>$</kbd> ${{\color{Cerulean}\small{\texttt{ 1 \quad}}}}$ <kbd>"</kbd> <kbd>_</kbd> ${{\color{Cerulean}\small{\texttt{ 001 }}}}$ <kbd>.</kbd> <kbd>"</kbd> ${{\color{Cerulean}\small{\texttt{ \quad}}}}$ <kbd>$</kbd> ${{\color{Cerulean}\small{\texttt{ 2 \quad}}}}$ <kbd>}</kbd> <kbd>'"'"'</kbd> `);`
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

<strong> My favorite error; basically, "Failure is not an option" X D</strong>

Now, to get to what I should really check for

(or "that for which I should really check.)

`  $` ${{\color{BrickRed}\texttt{ \quad grep \quad }}}$ 
<kbd>-</kbd> ${{\color{BrickRed}\texttt{i \quad}}}$
<kbd>"</kbd> ${{\color{DarkOrange}\texttt{failure}}}$ <kbd>"</kbd>
${{\color{BrickRed}\small{\texttt{ outfile001.out }}}}$
` | wc -l  # n_failures?`



## The complete section&mdash;having some helpful context&mdash;with keyboard-format highlights

Just in case the color doesn't render, and the previous section looks all
messed up.

```bash
$ cat >/dev/null <<EOF
#####################################################################
#  Going through one run of renaming files to my preferred format,
#+ used for al ~10^5 codex image files in the dataset.
#+ For some reason, when I use the PDF files from Universiteit 
#+ Utrecht (all open access), the image files extracted using
#+ a script I've put together called  `unwind_the_binding.py`
#+ end up with a different filename pattern than those from
#+ any other PDFs I've used. It keeps redownloading all them
#+ document images with slightly different filenames until them
#+ memory fills up.
#  Anyway, back to the different filename pattern ...
#+ Sounds like some good regexes will help.
#####################################################################
EOF
$ #  Keep track of stuff for repeatability
$ checksituation
$ #  What that command is (set up in ~/.bashrc and other sourced
$ #+ dotfiles). Note I can use tab completion
$ type checksituation
```

<kbd>checksituation is a function</kbd><br/>
<kbd>checksituation()</kbd><br/>
<kbd>{</kbd><br/>
` `<kbd>echo "   Current date/time:";</kbd><br/>
` `<kbd>if type trpdate > /dev/null 2>&1; then</kbd><br/>
`   `<kbd>trpdate;</kbd><br/>
` `<kbd>else</kbd><br/>
`   `<kbd>date;</kbd><br/>
`   `<kbd>date +'%s';</kbd><br/>
`   `<kbd>date +'%s_%Y-%m-%dT%H%M%S%z;</kbd><br/>
` `<kbd>fi;</kbd><br/>
` `<kbd>echo "   Current working directory:";</kbd><br/>
` `<kbd>pwd</kbd><br/>
<kbd>}</kbd><br/>

```bash
$ type trpdate
trpdate is aliased to `tripledate'
$ type tripledate
tripledate is aliased to `date && date +'%s' && date +'%s_%Y-%m-%dT%H%M%S%z''

$ : >>'EOF'
  First run-through gives files with <fname>_p0-<number>.png
  repeats (which fill memory if not stopped) are similar, but
  with  '_p1-', '_p2-', etc.
EOF
```

`  $ find . -type f -iname "*.png" | ` <kbd>grep "_p0-"</kbd> ` | wc -l # Unique image files`<br/>
`1025`<br/>
`  $ find . -type f -iname "*.png" | ` <kbd>grep -v "_p0-"</kbd> ` | wc -l # Number of duplicates`<br/>
`2324`<br/>
`  $` `#  We can double check`<br/>
`  $ find . -type f -iname "*.png" | ` <kbd>grep "_p1-"</kbd> ` | wc -l # Duplicated the first time`<br/>
`1025`<br/>
`  $ find . -type f -iname "*.png" | ` <kbd>grep "_p2-"</kbd> ` | wc -l # Duplicated the second time`<br/>
`1025`<br/>
`  $ find . -type f -iname "*.png" | ` <kbd>grep "_p3-"</kbd> ` | wc -l # Duplicated the third time`<br/>
`274`<br/>
`  $ find . -type f -iname "*.png" | ` <kbd>grep "_p4-"</kbd> ` | wc -l # Should have 0 duplctd 4th time`<br/>
`0`<br/>
`  $ ` `echo "1025+1025+274" | bc`
`2324`

<br/><hr/><br/>

```bash
$ find . -type f -iname "*.png" |
    xargs -I{} -0 bash -c '
      orig={};
      new_fname=$(echo "${orig}" |
```

`            `<kbd>awk -F '"'"'.'"'"' '"'"'{print $1 "_001." $2}'"'"'</kbd> `);`

```bash
      echo "  Renaming ${orig} to ${new_fname}"
      echo "       ...";
      mv "${orig}" "${new_fname}" && echo "           ... success" || echo "           ... FAILURE";
      echo
    ' 2>&1 | tee outfile001.out
```

`  $` <kbd>grep -i --failure outfile001.out</kbd> ` | wc -l # n_failures?`

<strong>`grep: unknown option -- failure`</strong>

```bash
Usage: grep [OPTION]... PATTERNS [FILE]...
Try 'grep --help' for more information.
0
```

<strong>My favorite error; basically, "Failure is not an option" X D</strong>

Now, to get to what I should really check for

(or "that for which I should really check.)

` $` <kbd>grep -i failure outfile001.out</kbd> ` | wc -l # n_failures?`

```bash
0

$ #  Great! No failures
```

## Some other favorite regexes (without colored/kbd annotation) in commands

### sed, simple and useful, and awk alternatives

<strong>More-readable filename list</strong>

```bash
find . -type f -iname "*.png | sed 's:^[.]/::g;' | sort > rename_list.lst

find . -type f -iname "*.png | awk -F'/' '{print $NF}' | sort > rename_list.lst
```

<strong>Isolating parts of file by line numbers</strong>

```bash
# best for shorter files
sed -n '51,100p' file_with_250_entries.txt > second_of_five_fifties.txt 

# best for longer files
sed -n '4735, 5221p;5222q' thirty_one_k_line_bible.txt > favorite_chapter.txt

# Okay with short or long files, example with long
awk 'NR==51,NR==100' | file_with_250_entries.txt > second_of_five_fifties.txt
```


### just sed

<strong>Back up and remove some problematic lines

```bash
sed -i.bak '195,201d' this_script.sh
```

<strong>Remove stray backslashes that got added as a first line with a strange copy/paste</strong>

```bash
sed -i '1{/^\\$/d}'
```

### sed and grep

<strong>Make a creative version of `tree`, which excludes folders but gives their names and an `(excluded)` marker</strong>

This is one of my favorites, though it might be hard to follow. I manually put in the directory names.

```bash
echo "$(realpath .)" && \
  tree --noreport --charset=ascii -f . |
    grep -vP "(outputs|system_abt_1756965227_-_try_1|bash_terminal_io_lab_notebooks)/" |
    sed 's:^\([^/]\+\)[.]/.*/\([^/]\+\)$:\1\2:g; s:^\([^/]\+\)[.]/\([^/]\+\)$:\1\2:g;' |
    sed 's:\(outputs\|system_abt_1756965227_-_try_1\|bash_terminal_io_lab_notebooks\):&/ (excluded):g; '\
's:\(datasets\|cifar-10-batches-py\|logs\|models\|notebooks\|scripts\|visualizations\):&/:g; s:^[.]$::g;' |
    awk 'NF'
```

<strong>Example use</strong>

Here, you'll get to see my bash prompt, too. It's a longer command than the one before; that's just to give you the idea.

```bash


```

### tasks that come up often

<strong>Removing blank lines</strong>

```bash
# without space
sed '/^$/d' file_with_really_blank_lines.csv > without_byteless_lines.csv 
tr -s '\n' < file_with_really_blank_lines.csv > without_byteless_lines.csv 
awk 'NF' file_with_really_blank_lines.csv > without_byteless_lines.csv 
grep -v "^.+$" file_with_really_blank_lines.csv > without_byteless_lines.csv 

# with spaces (simple)
sed '/^[[:space:]]*$/d' file_with_difft_types_blank_lines_lines.csv > no_whitespace_lines.csv
awk 'NF' file_with_difft_types_blank_lines_lines.csv > no_whitespace_lines.csv
grep -v '^[[:space:]]*$' file_with_difft_types_blank_lines_lines.csv > no_whitespace_lines.csv
grep -Pv '^[ \t\r\n\v\r]*$' file_with_difft_types_blank_lines_lines.csv > no_whitespace_lines.csv


# Okay with short or long files, example with long
awk 's#^[.]/##g;' | sort > rename....sh
```

<strong>Counting instances of each file extension, making sure all files have an extension</strong>

```bash
# count num_files in directory
num_files_1=$(find . -type f | wc -l)
:' Example output
17
'

# find number of files with each extension (from most to lest)
find . -type f | awk -F'.' '{print $NF}' | sort | uniq -c | sort -rn
:' Example output:
   8 txt
   3 md
   3 png
   2 py
   1 sh
'

# Sum up the counts for each extension
find . -type f | awk -F'.' '{print $NF}' | sort |
  uniq -c | sort -rn | awk '{sum += $1}; END { print sum }' 
:' Example output
17
'

