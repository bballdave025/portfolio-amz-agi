# Some simple Regular Expression usage and other scripting in `bash` 

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

## The regex part

There were plans to have some [seriously color-coded commands and regexes&mdash;[example 1](https://github.com/bballdave025/portfolio-amz-agi/blob/main/isolated_examples/regex_deeper.md) & [example 2](https://github.com/bballdave025/fhtw-paper-code-prep/blob/main/dataset_preparation_examples/steps_rename_utrecht.md), but I decided becoming an expert in LaTeX typography wasn't my first priority.

Those commands which I especially want to highlight will be in <kbd>keyboard style</kbd> instead of <code>code style</code>. These include regex usage and general `bash` usage`

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

