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
  this Colab notebook
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

<strong>Back up and remove some problematic lines</strong>

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

Here, you'll get to see my bash prompt, too. It's a longer command than the one before; that's just to give you the idea. It's also easily metaprogrammed, and I'm working on automating it.

```bash
 <=> conda environment, blank means none activated
[cifar10-vanilla-cnn] <=> git branch, blank means not in a git repo
bballdave025@MY-MACHINE ~/my_repos_dwb/fhtw-paper-code-prep
$ echo && echo -e "$(realpath .)" && tree --noreport --charset=ascii -f . | grep -vP "(outputs|system_abt_1756965227_-_try_1|bash_terminal_io_lab_notebooks|backups|cifar-10-batches-py|p_01|__pycache__|environment_specifications|system_1757194639_2025-09-06T153719-0600|img|dataset_preparation_examples|logs|general_lab_notebooks_other_examples)/" | sed 's:^\([^/]\+\)[.]/.*/\([^/]\+\)$:\1\2:g; s:^\([^/]\+\)[.]/\([^/]\+\)$:\1\2:g;' | sed -E 's:(outputs|system_abt_1756965227_-_try_1|bash_terminal_io_lab_notebooks|backups|cifar-10-batches-py|p_01|__pycache__|environment_specifications|system_1757194639_2025-09-06T153719-0600|img|dataset_preparation_examples|logs|general_lab_notebooks_other_examples)$:&/ (excluded):g; s:(datasets|models|notebooks|scripts|visualizations|p_03_e2e|datasets|tests)$:&/:g; s:^[.]$::g;' | grep -v "Untitled\|bak" | awk 'NF'

/home/bballdave025/my_repos_dwb/fhtw-paper-code-prep
|-- apply_local_fixes_1757283644_2025-09-07T162044-0600.sh
|-- backups/ (excluded)
|-- bin
|   |-- audit_backup.sh
|   |-- backup_everything.sh
|   |-- backup_notebooks.sh
|   |-- dwb_selective_tree.ps1
|   |-- elapsed.sh
|   |-- fix_firstline_glitches.sh
|   |-- foo_safe_local_header.sh
|   |-- helpers.sh
|   |-- safe_default_header.sh
|   |-- start_cifar_lab.sh
|   |-- sys_capture.sh
|   `-- system_info_bash_fallbacks.sh
|-- dataset_preparation_examples/ (excluded)
|-- datasets/
|-- environment_specifications/ (excluded)
|-- general_lab_notebooks_other_examples/ (excluded)
|-- img/ (excluded)
|-- LICENSE
|-- logs/ (excluded)
|-- Makefile
|-- notebooks/
|   `-- 02_training_fhtw-paper-code-prep.ipynb
|-- NOTWORKING_structure.bat
|-- outputs/ (excluded)
|-- Paper_Code_Prep_01-00-00_CNN_GradCAM_TF.ipynb
|-- Paper_Code_Prep_01.pdf
|-- README.md
|-- revert_local_fixes_1757283644_2025-09-07T162044-0600.sh
|-- scripts/
|   |-- py_utils_fhtw-paper-code-prep.py
|   `-- tests/
|       `-- test_elapsed_all.sh
|-- structure.ps1
|-- structure.sh
|-- test_project_bash
|   |-- p_01/ (excluded)
|   `-- p_03_e2e/
|       |-- datasets/
|       |   `-- datasets/
|       |       |-- cifar-10-batches-py/ (excluded)
|       |       `-- cifar-10-batches-py.tar.gz
|       |-- __init__.py
|       |-- logs/ (excluded)
|       |-- models/
|       |-- notebooks/
|       |   |-- 00_data_exploration_p_03_e2e.ipynb
|       |   |-- 01_model_build_p_03_e2e.ipynb
|       |   |-- 02_training_p_03_e2e.ipynb
|       |   |-- 03_inference_quick_explore_p_03_e2e.ipynb
|       |-- outputs/ (excluded)
|       |-- README_p_03_e2e.md
|       |-- scripts/
|       |   |-- build_model_p_03_e2e.cmd
|       |   |-- build_model_p_03_e2e.ps1
|       |   |-- build_model_p_03_e2e.sh
|       |   |-- inference_p_03_e2e.cmd
|       |   |-- inference_p_03_e2e.ps1
|       |   |-- inference_p_03_e2e.sh
|       |   |-- __init__.py
|       |   |-- normalize_eol.py
|       |   |-- py_build_model_p_03_e2e.py
|       |   |-- __pycache__/ (excluded)
|       |   |-- py_inference_p_03_e2e.py
|       |   |-- py_touch.py
|       |   |-- py_train_model_p_03_e2e.py
|       |   |-- py_utils_p_03_e2e.py
|       |   |-- py_utils_p_03_e2e.sh
|       |   |-- train_model_p_03_e2e.cmd
|       |   |-- train_model_p_03_e2e.ps1
|       |   `-- train_model_p_03_e2e.sh
|       `-- visualizations/
`-- validate_env.py
=====..........................................................retval=0.........

 <=> conda environment, blank means none activated
[cifar10-vanilla-cnn] <=> git branch, blank means not in a git repo
bballdave025@MY-MACHINE ~/my_repos_dwb/fhtw-paper-code-prep
$
```

I still have a bit of clean-up to do. How nice is it that the building of the structure and
the building of scripts and notebooks reusable in other ML/AI projects is automated, as is
the launching of Jupyter notebooks!

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
```

## Some more advanced usage (not always as long, though)

Most of these come from 
[this log](https://github.com/bballdave025/fhtw-paper-code-prep/blob/main/dataset_preparation_examples/guide_example_for_ensuring_filename_characters_are_okay.log) 
or 
[that log](https://github.com/bballdave025/fhtw-paper-code-prep/blob/main/dataset_preparation_examples/Lab_Notebook_bballdave025_1755369231_2025-08-16T123351-0600_clean.log). 
They're from an older prompt version (well, from before I re-set-up my prompt).

### Cleansing the filenames in my database to make parsing easier

```bash
bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "*.jpg" | awk -F'/' '{print $NF}' | LC_ALL=C grep -P "[\x80-\xFF]" # Non-ascii
FamilySearch_-_DGS Бъ 008268002_00568_scg.jpg
Besançon Bibliothèqe Municiπale_-_color-MS¶0716_00122_mbr_ucr_orc.jpg
FamilySearch="iħm xm ⁊ d"_-_DGS004459005_00069_mbr_mcl_fmr_cwa.jpg
BiblioþequesMediathequesDeMetz_-_Ms № 464_00092_0047B_oic_nbr.jpg
BnF-BiɓArsenal_-_Ms-87_00203_oic_nbr.jpg
Bodleian-Library?-MS-(AddAæ)-263°_00137_oic_nbr.jpg
FamilySearcℏ_-_DGS004763803_00695_oic_nbr.jpg
Family~Search_-_DGS005141812-ꝯ_00004_oic_nbr.jpg
FamilɤSearchɟ_—_DGS004321721&_00066@_oic_nbr.jpg
UnivBologna-SistemaMusealed'Ateneo-AlmaMaterStudiorum_-_ErbarioAldrovandi-Vol-ᴠɪ-60001_orc.jpg
FamilySearch_-_DGS102439140☞00014_ucr_suh_mcl.jpg

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]"
▒
▒
▒

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  -^- Helpful just in knowing some non-ASCII codepoints are there. We  grep-ped  for one byte
#+ and any non-ASCII are 2+ bytes (in UTF-8)

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n' | wc -c # This will tell us the number of _bytes_, not generally the number of _characters_
3

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n'
ℏ
bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n' | sed 's#\(.\)#'"'"'\1'"'"', #g; s#, $##g;'
'ℏ'
bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  REMEMBER TO CHANGE THE `-iname' STRING if copying and pasting

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ ( find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n' | sed 's#\(.\)#\1.#g;' > my_tmp_bytes ) && while IFS= read line; do this_character="${line}"; printf "%s" "${line}" | xxd; done < <(sed 's#\n#--linefeed--#g' my_tmp_bytes | tr '.' '\n' | sed 's#--linefeed--#\n#g') && rm my_tmp_bytes
00000000: e284 8f                                  ...

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  What I just did there tells me I should make it a function, or at least
#+ have a place to change the string at the beginning. (What I did there was
#+ to write the REMEMBER TO CHANGE ... STRING
#+ not to mention writing "copy ... paste")
#+
#+   (Actually, however, escaping the wildcards for the globbing would make 
#+    such a function quite difficult to get right.)

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  We have just one character, on its own line. THIS IS NOT NECESSARY EVERY
#+ TIME, but to be more clear,

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  (REMEMBER TO CHANGE THE `-iname' STRING if copy/paste-ing)

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ ( find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n' | sed 's#\(.\)#\1.#g;' > my_tmp_bytes ) && while IFS= read line; do this_character="${line}"; printf "%s" "${line}" | xxd; done < <(sed 's#\n#--linefeed--#g' my_tmp_bytes | tr '.' '\n' | sed 's#--linefeed--#\n#g') | sed 's#^[0-9a-f]\+[:]##g; s#[^0-9a-f]##g; s#\(\(.\)\{2\}\)#\1 #g; s# $##g; s#^#0x#g; s#[ ]# 0x#g' && rm my_tmp_bytes
0xe2 0x84 0x8f

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  There we go. One byte-encoded UTF-8 codepoint (glyph), which we
#+ often call a character. Don't do this every time, but checking ...

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ echo -e "\xe2\x84\x8f"
ℏ

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {}  # Looking for a single quote. This also gives us the classification dir
./No_For_Binding_Reuse/FamilySearcℏ_-_DGS004763803_00695_oic_nbr.jpg

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  REMEMBER THE CLASSIFICATION DIRECTORY,
#+ be cautious by using `mv -i',
#+ and also make sure to check that the first (now-existing) file 
#+ will tab-complete.
#+ If there were single quotes in the `ls' version or the `%q' version,
#+ you'll likely need them (or backslash escapes) for the rename.

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ mv -i ./No_For_Binding_Reuse/FamilySearcℏ_-_DGS004763803_00695_oic_nbr.jpg ./No_For_Binding_Reuse/FamilySearch_-_DGS004763803_00695_oic_nbr.jpg  # the first filename tab-completed
```

## Checking for consistency in classification

This one is easier to explain with images.

An image cannot have No Binding Reuse (`nbr`)



and a Multiple Class Reuse (`mcl`), meaning at least one type of binding reuse as
well as something else interesting.



at the same time. It can't be `A` and `not A` at the same time

```bash
bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ head uniq_combo_sorted_legals_1754293729_2025-08-04T014849-0600.lst
abg cwa
abg cwa fko fmr mbr ucr
abg cwa fmr mbr mcl
abg cwa fmr mbr scg suh
abg cwa fmr mbr tbr
abg cwa fmr mbr ucr
abg cwa mbr tbr
abg fko fmr
abg fko fmr mbr ucr
abg fko gni mbr mcl

bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ my_ts="$(ttdate)"

bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ grep "mcl" uniq_combo_sorted_legals_1754293729_2025-08-04T014849-0600.lst > mcl_in_uniq_combo_sorted_legals_${my_ts}.lst

bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ head mcl_in_uniq_combo_sorted_legals_1754293729_2025-08-04T014849-0600.lst
abg cwa fmr mbr mcl
abg fko gni mbr mcl
abg fmr iac mcl
abg fmr mbr mcl
abg fmr mbr mcl scg
abg fmr mcl
abg mbr mcl suh tbr ucr
abg mcl tbr
cwa fko fmr mbr mcl
cwa fko mbr mcl scg

bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ grep -v "mbr" mcl_in_uniq_combo_sorted_legals_1754293729_2025-08-04T014849-0600.lst |
    grep -P '(?<! (cwa|fmr|gni|nbr|orc|scg|spr|tbr|ucr))'\
'( (cwa|fmr|gni|nbr|orc|scg|spr|tbr|ucr))'\
'(?! (cwa|fmr|gni|nbr|orc|scg|spr|tbr|ucr))'
 abg fmr iac mcl
 abg fmr mcl
 abg mcl tbr
 cwa mcl
 fko fmr mcl
 fko gni mcl
 fko mcl nbr
 fko mcl scg
 fko mcl tbr
 fko mcl ucr
 fmr mcl
 fmr mcl ucr
 gni mcl
 iac mcl ucr
 mcl orc
 mcl scg
 mcl spr
 mcl suh ucr
 mcl tbr
 mcl ucr

bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ cat files_to_rectify_1754293729_2025-08-04T014849-0600.list
# '.' is '/cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/'\
#'Classified_-_Almost_Just_3lett_FS_-_Combined'
./No_For_Binding_Reuse/SolothurnDomschatzStUrsenKath_-_SilverEvangelary_CodU2-ecod_00002_mcl_fko_nbr.jpg

bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ grep -v "mbr" mcl_in_uniq_combo_sorted_legals_1754293729_2025-08-04T014849-0600.lst | grep -vP '(?<! (cwa|fmr|gni|orc|scg|spr|tbr|ucr).{0,251})( (cwa|fmr|gni|orc|scg|spr|tbr|ucr))(?!.* (cwa|fmr|gni|orc|scg|spr|tbr|ucr))'
 fko mcl nbr
 fmr mcl ucr

bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "*.jpg" | grep -P "(?=.*_nbr)(?=.*_fko)(?=.*_mcl)" | wc -l
1

bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "*.jpg" | grep -P "(?=.*_nbr)(?=.*_fko)(?=.*_mcl)"
./No_For_Binding_Reuse/SolothurnDomschatzStUrsenKath_-_SilverEvangelary_CodU2-ecod_00002_mcl_fko_nbr.jpg

bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #+ (Offset stuff was likely previously glued, as seems to be the case here, with glue-ey marks by some text and some thin paper still glued.)

bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f | grep SolothurnDomschatzStUrsenKath_-_SilverEvangelary_CodU2-ecod_00002_mcl_fko_nbr.jpg
./No_For_Binding_Reuse/SolothurnDomschatzStUrsenKath_-_SilverEvangelary_CodU2-ecod_00002_mcl_fko_nbr.jpg

bballdave025@MY_MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ mv ./No_For_Binding_Reuse/SolothurnDomschatzStUrsenKath_-_SilverEvangelary_CodU2-ecod_00002_mcl_fko_nbr.jpg ./No_For_Binding_Reuse/SolothurnDomschatzStUrsenKath_-_SilverEvangelary_CodU2-ecod_00002_mcl_fko_fmr.jpg
```
