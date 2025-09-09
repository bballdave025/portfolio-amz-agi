# Some simple Regular Expression usage and other scripting in `bash` 

## Background (a skippable section)

These examples are part of the dataset preparation for my classification of different types of
of Reused Manuscript Fragments in Bindings (RMFB). 

[Skip to the regex part](#The-regex-part)

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

<br/>

The examples I show here came specifically from working of the conversion of
public-domain/open-access PDF documents containing images of old books, many of which are 
manuscripts on parchment (animal skin treated for writing) or paper contemporary to the 
parchment. There is also printed material. This is part of a process document (step-by-step
reminder) for going through the process quickly and preparing more and more automation. I 
also show some of the patterns I often use in `bash` scripting, including the use of `find` 
and several file-handling operations. Here, I have used some markdown to highlight certain
commands and processes that were originally part of my 
[terminal logfile]() for the day.

## The regex part

The specific usage examples will be colored, actually highlighted,for which I need to use 
MathJax (LaTeX) and thus won't have a <code>code</code> font. (Solutions people had tried, 
such as `<code style="color:red">whatever code</code>` haven't worked in summer 2025.)

<code>I'll put my </code> ${{\color{Cerulean}\small{\texttt{ \quad awk \quad code \quad }}}}$ <code> in </code> ${{\color{Cerulean}\small{\texttt{ \quad blue \quad }}}}$

<code>I'll put my (not-specifically-regex) </code> ${{\color{BrickRed}\small{\texttt{ \quad grep \quad code \quad }}}}$ <code> in </code> ${{\color{BrickRed}\small{\texttt{ \quad red \quad }}}}$

<code>I'll put my (not-specifically-regex) </code> ${{\color{ForestGreen}\small{\texttt{ \quad sed \quad code \quad }}}}$ <code> in </code> ${{\color{ForestGreen}\small{\texttt{ \quad green \quad }}}}$

<code>I'll put my </code> ${{\color{DarkOrange}\small{\texttt{ \quad code \quad for \quad regexes \quad }}}}$ <code> in </code> ${{\color{DarkOrange}\small{\texttt{ \quad orange \quad }}}}$

This will be the pattern for the commands themselves.

For the regex part, I'll use <span style="background-color: yellow;">highlight text</span> blah

<br/>







