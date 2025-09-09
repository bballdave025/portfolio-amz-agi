Okay, I have spent too much time trying to get the others all formatted to highlight stuff. I'm just hoping ChatGPT can look in here, 
figure out the basics, and pull out some good examples of `grep` and `sed` with enough context to understand, and also some `awk` and 
even `find` stuff that looks good

I'm just including the logfile/instructions for the day that I went through a lot of this stuff. This one is mostly just instructions&mdash;not much output.

It comes from [here](https://github.com/bballdave025/fhtw-paper-code-prep/edit/main/dataset_preparation_examples/steps_rename_utrecht.log)

```bash

###################################
##  go down to the part with     ##
## HERE IS WHERE YOU WANT TO GO! ##
###################################

##############################################################################
##############################################################################
##############################################################################
####                                                                      ####
#                HERE IS WHERE YOU WANT TO GO                                #
####                                                                      ####
##############################################################################
##############################################################################
##############################################################################







$ cat >/dev/null <<'EOF'
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
$ #
#  What that command is (set up in ~/.bashrc and other sourced
#+ dotfiles). Note I can use tab completion
$ type checksituation
checksituation is aliased to `echo; echo " Current date/time is"; trpdate; echo; echo " Current directory ( pwd ) is "; pwd; echo;'
$ type trpdate
trpdate is aliased to `tripled  ate'
$ type tripledate
tripledate is aliased to `date && date +'%s' && date +'%s_%Y-%m-%dT%H%M%S%z''
$ find ./new_to_convert_PDF/ -type f | wc -l # Making sure not tons of output
$ find ./new_to_convert_PDF/ -type f
$ find ./converted_from_PDF/ | wc -l
$ find ./converted_from_PDF/
$ #  just the directory, itself.
$
$ #  Let's make a nice, multi-line comment in an easy way
cat >/dev/null <<EOF
In Anaconda Prompt (for this example, I'm in Windows.)

path>:: Starting near <timestamp from checksituation>
path>python unwind_the_binding.py
##OUTPUT

Default usage looks for PDF in 'new_to_convert_PDF'
and outputs into 'converted from PDF'

(Note that my  bash  is from Cygwin, and I'm returning
to typing this multi-line comment there.)

EOF

$ checksituation

$ cat >/dev/null <<'EOF'
After a [Ctrl]+[c] in Anaconda Prompt, after I can see
that we're downloading repeat/duplicate images with
the script.

path>:: blah

EOF

$ #
#  First run-through gives files with <fname>_p0-<number>.png
#+ Repeats give similar, but with '_p1-', '_p2-', etc. 

$ cd converted_from_PDF
$ # 
# complete_path="/cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/"\
#"new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF/"
# You can make sure the directory is right with the next checksituation
$ checksituation
$ find . -type f | wc -l
$ find . -type f -iname "*.png" | wc -l  # all files should be PNG, for now
$ find . -type f -iname "*.png" | \
    grep "_p0-" | \
      wc -l
$ #  Matches the count from the image extraction
$ find . -type f -iname "*.png" | \
    grep -v "_p0-" | \
      wc -l
$ #  That's the number of duplicates. Double checking
$ find . -type f -iname "*.png" | \
    grep "_p1-" | \
      wc -l
$ find . -type f -iname "*.png" | \
    grep "_p2-" | \
      wc -l
$ find . -type f -iname "*.png" | \
    grep "_p2-" | \
      wc -l
$ cat >/dev/null <<'EOF'

Let's delete the duplicates. 
Null separating filenames in case there are any spaces, etc.
in any filenames. (There aren't, but I want it portable for
when I'm using filenames created by others.)

EOF
$ find . -type f -iname "*.png" | \
    grep -v "_p0-" | \
      tr '\n' '\0' | \
        xargs -I'{}' -0 rm "{}"
$ find . -type f -iname "*.png" | \
    grep -v "_p0-" | \
      wc -l #  should be zero (0)
$ #  They're all gone. Let's check for what's left
$ find . -type f -iname "*.png" | \
    grep "_p0-" | \
      wc -l
$ #  Matches. Let's take a quick look at the filenames
$ find . -type f -iname "*.png" | sort | head
#<UnivUltrecht_-_F-oct-39-dl-1>
$ #
#  Make the filenames consistent with my others
#+ I'll be metaprogramming a script to do this
$ #find . -type f -iname "*.png | sed 's#^[.]/##g;' | sort > rename....sh
$   # probably more compute with regex engine, less robust
$ find . -type f -iname "*.png" | \
    awk -F'/' '{print $NF}' | \
      sort | wc -l
$ #
#  Make sure we don't have anything weird with the sorted output;
#+ something weird happens when there are more than 1000 images, e.g.
#+ what's in Appendix 1
$ #
#  Actually, the problems happen when the _p0-<number> has numbers that
#+ straddle 999 and 1000 [or any (10^k - 1) and (10^k), k>2
#+ Well, theoretically, for any k, I think ...
$ find . -type f -iname "*.png" | \
    awk -F'/' '{print $NF}' | \
      sort | head
$ find . -type f -iname "*.png" | \
    awk -F'/' '{print $NF}' | \
      sort | tail
$ find . -type f -iname "*.png" | \
    awk -F'/' '{print $NF}' | \
      sort > rename_utrecht_foct39dl1_pre.sh
$ head rename_utrecht_foct39dl1_pre.sh
$ cat >/dev/null <<'EOF'

  Metaprogramming (running something that will output a script)
+ First testing my long command by outputting to stdout (not to
+ script file) and not running any file-changing commands (such
+ as rm, mv, etc.) Not running tee to filename or anything
+ like that.

EOF
$
$ cat >/dev/null <<'EOF'

#  Here are the commands from a previous big rename.
#+ I change things around. This could be automated, but
#+ I think getting the automation (a meta-meta-programming
#+ script) done would take longer than the combined
#+ time to manually make changes.

curr_idx=1; ofn="rename_utrecht_eoct416dl1.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_eoct416dl1.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" | tee -a "${ofn}"; echo "echo \"  Moving\"" | tee -a "${ofn}"; echo "echo \"${orig_fname}\"" | tee -a "${ofn}"; echo "echo \"  TO\"" | tee -a "${ofn}"; echo "echo \"${new_fname}\"" | tee -a "${ofn}"; echo "echo \"      ...\"" | tee -a "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" | tee -a "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_eoct416dl1_pre.sh

curr_idx=1; ofn="rename_utrecht_eoct416dl1.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_eoct416dl1.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" >> "${ofn}"; echo "echo \"  Moving\"" >> "${ofn}"; echo "echo \"${orig_fname}\"" >> "${ofn}"; echo "echo \"  TO\"" >> "${ofn}"; echo "echo \"${new_fname}\"" >> "${ofn}"; echo "echo \"      ...\"" >> "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" >> "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_eoct416dl1_pre.sh

#  this time: rename_utrecht_foct39dl1_pre.sh, rename_utrecht_foct39dl1.sh

EOF
$ #
#########################################################
#########################################################
## There probably won't, but there's a possibility there
## might be changes necessary for the  sed  , based on
## stuff from Appendix 1. If you didn't use Appendix 1,
## don't worry about this.
## (If you _did_ use Appendix 1, there probably won't be
## a change either, but there is a possibility.)
#########################################################
#########################################################

$ #  Copy/paste into here and from here
$ cat >/dev/null <<'EOF'

##################### PREVIOUS ##########################

#  Checking that the script will look like we want it to look.
#+ The preview command.
curr_idx=1; ofn="rename_utrecht_lfol802.sh"; >"${ofn}"; \
while read -r line; do \
  orig_fname="${line}"; \
  ofn="rename_utrecht_lfol802.sh"; \
  new_fname_pre=$(echo "${orig_fname}" | \
        sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); \
  new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; \
  echo "echo"; \
  echo "echo \"  Moving\""; \
  echo "echo \"${orig_fname}\""; \
  echo "echo \"  TO\""; \
  echo "echo \"${new_fname}\""; \
  echo "echo \"      ...\""; \
  echo "mv \"${orig_fname}\" \"${new_fname}\" \
    && echo \"          ... success\" \
    || echo \"          ... FAILURE\""; 
  curr_idx=$(echo "${curr_idx}+1" | bc); 
done < rename_utrecht_lfol802_pre.sh | \
                                     head -n 50

#  Saving the commands to a script that we'll run.
#+ (put in the appropriate  ` >> "${ofn}"'
curr_idx=1; ofn="rename_utrecht_lfol802.sh"; >"${ofn}"; \
while read -r line; do \
  orig_fname="${line}"; \
  ofn="rename_utrecht_lfol802.sh"; \
  new_fname_pre=$(echo "${orig_fname}" | \
        sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); \
  new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; \
  echo "echo" >> "${ofn}"; \
  echo "echo \"  Moving\"" >> "${ofn}"; \
  echo "echo \"${orig_fname}\"" >> "${ofn}"; \
  echo "echo \"  TO\"" >> "${ofn}"; \
  echo "echo \"${new_fname}\"" >> "${ofn}"; \
  echo "echo \"      ...\"" >> "${ofn}"; \
  echo "mv \"${orig_fname}\" \"${new_fname}\" \
    && echo \"          ... success\" \
    || echo \"          ... FAILURE\"" >> "${ofn}"; 
  curr_idx=$(echo "${curr_idx}+1" | bc); 
done < rename_utrecht_lfol802_pre.sh

EOF

cat >/dev/null <<'EOF'

####################### CURRENT ############################
#
# Copy/paste from above, then change what's needed (for
# the first one, usually just) 
#    rename_utrecht_<change-this>_pre.sh
#    rename_utrecht_<change-this>.sh
# Then, for the second one, you could do the same
# changes, but I prefer to work off the working preview
# that we saw for the first one. So, I do this ...
#     Copy the preview
#     Put in the "appending to the outfile name" part:
#             ` >> "${ofn}"'
#       on the same lines as shown in the second example
#       script above (i.e. in the same places).
#

EOF

cat >/dev/null <<'EOF'

# First
#  this time: rename_utrecht_foct39dl1_pre.sh, rename_utrecht_foct39dl1.sh

#
#  Checking that the script will look like we want it to look.
#+ The preview command.
curr_idx=1; ofn="rename_utrecht_foct39dl1.sh"; >"${ofn}"; \
while read -r line; do \
  orig_fname="${line}"; \
  ofn="rename_utrecht_foct39dl1.sh"; \
  new_fname_pre=$(echo "${orig_fname}" | \
        sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); \
  new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; \
  echo "echo"; \
  echo "echo \"  Moving\""; \
  echo "echo \"${orig_fname}\""; \
  echo "echo \"  TO\""; \
  echo "echo \"${new_fname}\""; \
  echo "echo \"      ...\""; \
  echo "mv \"${orig_fname}\" \"${new_fname}\" \
    && echo \"          ... success\" \
    || echo \"          ... FAILURE\""; 
  curr_idx=$(echo "${curr_idx}+1" | bc); 
done < rename_utrecht_foct39dl1_pre.sh | \
                                     head -n 50

EOF

cat >/dev/null <<'EOF'

# Second
#  this time: rename_utrecht_foct39dl1_pre.sh, rename_utrecht_foct39dl1.sh

#
#  Saving the commands to a script that we'll run.
#+ (put in the appropriate  ` >> "${ofn}"'
curr_idx=1; ofn="rename_utrecht_foct39dl1.sh"; >"${ofn}"; \
while read -r line; do \
  orig_fname="${line}"; \
  ofn="rename_utrecht_foct39dl1.sh"; \
  new_fname_pre=$(echo "${orig_fname}" | \
        sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); \
  new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; \
  echo "echo" >> "${ofn}"; \
  echo "echo \"  Moving\"" >> "${ofn}"; \
  echo "echo \"${orig_fname}\"" >> "${ofn}"; \
  echo "echo \"  TO\"" >> "${ofn}"; \
  echo "echo \"${new_fname}\"" >> "${ofn}"; \
  echo "echo \"      ...\"" >> "${ofn}"; \
  echo "mv \"${orig_fname}\" \"${new_fname}\" \
    && echo \"          ... success\" \
    || echo \"          ... FAILURE\"" >> "${ofn}"; 
  curr_idx=$(echo "${curr_idx}+1" | bc); 
done < rename_utrecht_foct39dl1_pre.sh



#  (After, we'll have a look at the script we've outputted, i.e.
#       rename_utrech_<this>.sh
#  to make sure we wrote all of the right lines.

EOF

$ cat >/dev/null <<'EOF'

#  Note that this will allow us to see either 'success' or 'FAILURE'
#+ output for each rename. Since it's redirected to an output file,
#+ we can check if there are any failures and count the successes.
#+ More on that, later.

EOF
$ ##  REMEMBER TO CHANGE THE SHELFMARK!!! ##
$ ##+ (using tab-complete is the way to go, here.
$ head rename_utrecht_foct39dl1.sh
$ rm rename_utrecht_foct39dl1_pre.sh
$ #  Finishing the script (and showing it)
$ cp rename_utrecht_foct39dl1.sh ru_bak
$ echo -e "\x23\x21/usr/bin/env bash\n\n" # check
$ echo -e "\x23\x21/usr/bin/env bash\n\n" > first_part
$ cat first_part rename_utrecht_foct39dl1.sh > tmpf \
       && mv tmpf rename_utrecht_foct39dl1.sh
$ diff rename_utrecht_foct39dl1.sh ru_bak
$ rm first_part
$ rm ru_bak
$ chmod a+x rename_utrecht_foct39dl1.sh  # make it executable
$ #
#  I'm teeing the output to a progress file and to stdout.
#+ that will allow me to see if there are a bunch of FAILUREs,
#+ in which case I could [Ctrl]+[c]
$ ./rename_utrecht_foct39dl1.sh 2>&1 | tee renaming_utrecht_foct39dl1.out

### I've taken out most of the output

### ... OUTPUT ...

$ #  Now, check for any FAILUREs, using case-insensitive flag
$ grep -i --failure renaming_utrecht_foct39dl1.out | wc -l # n_failures?
grep: unknown option -- failure
Usage: grep [OPTION]... PATTERNS [FILE]...
Try 'grep --help' for more information.
0

$ #
#  hahahahaha. I've got to make sure I laugh every day.
#+ Failure is not an option ('unknown option -- failure')
$ grep -i failure renaming_utrecht_foct39dl1.out | wc -l # n_failures
$ #  Great! No failures. Check successes.
$ grep -i success renaming_utrecht_foct39dl1.out | wc -l
$ #  Matches our original number of files; good
$
$ #  A regex check that all filenames match my favored pattern
$ find . -type f -iname "*.png" | wc -l
$ find . -type f -iname "*.png" | sort | head
$ find . -type f -iname "*.png" | sort | \
    grep "_[0-9]\{5\}[.]png$" | 
    wc -l
$ find . -type f -iname "*.png" | sort | \
    grep -v "_[0-9]\{5\}[.]png$" | \
    wc -l






















#
##############################################################################
# Appendix 1 #
##############

#######################
# NOTE POSSIBLE SITUATIONS LIKE THE FOLLOWING
#
# ALWAYS RUN THIS CHECK FIRST, EVEN BEFORE
# CHECKSITUATION (maybe)

$ cat >/dev/null <<'EOF'

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ find . -type f -iname "*.png" | \
    grep -o "_p0-[0-9]" | \
      sort | uniq -c | sort -rn | wc -l
5

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ find . -type f -iname "*.png" | \
    grep -o "_p0-[0-9]" | \
      sort | uniq -c | sort -rn
    134 _p0-1
     50 _p0-9
     50 _p0-8
     50 _p0-7
     31 _p0-6

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$


THE SED COMMANDS WILL NEED TO CHANGE, SOMETHING LIKE

sed 's#_p0-\([6-9]\)#_p0-0\1#g;'

EOF


$ checksituation

 Current date/time is
Mon Jun 30 15:08:42 MDT 2025
1751317722
1751317722_2025-06-30T150842-0600

 Current directory ( pwd ) is
/cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF_-_extra_01


$ find . -type f -iname "*.png" | \
    awk -F'/' '{print $NF}' | \
    sort | wc -l
470
$ find . -type f -iname "*.png" | \
    awk -F'/' '{print $NF}' | \
      sort | head
UnivUtrecht_-_L-fol-802_p0-1001.png
UnivUtrecht_-_L-fol-802_p0-1003.png
UnivUtrecht_-_L-fol-802_p0-1005.png
UnivUtrecht_-_L-fol-802_p0-1007.png
UnivUtrecht_-_L-fol-802_p0-1009.png
UnivUtrecht_-_L-fol-802_p0-1011.png
UnivUtrecht_-_L-fol-802_p0-1013.png
UnivUtrecht_-_L-fol-802_p0-1015.png
UnivUtrecht_-_L-fol-802_p0-1017.png
UnivUtrecht_-_L-fol-802_p0-1019.png

$ find . -type f -iname "*.png" | \
    awk -F'/' '{print $NF}' | \
      sort | tail
UnivUtrecht_-_L-fol-802_p0-981.png
UnivUtrecht_-_L-fol-802_p0-983.png
UnivUtrecht_-_L-fol-802_p0-985.png
UnivUtrecht_-_L-fol-802_p0-987.png
UnivUtrecht_-_L-fol-802_p0-989.png
UnivUtrecht_-_L-fol-802_p0-991.png
UnivUtrecht_-_L-fol-802_p0-993.png
UnivUtrecht_-_L-fol-802_p0-995.png
UnivUtrecht_-_L-fol-802_p0-997.png
UnivUtrecht_-_L-fol-802_p0-999.png

$ find . -type f -iname "*.png" | \
    awk -F'/' '{print $NF}' | \
      sort | less

$ find . -type f -iname "*.png" | \
    awk -F'/' '{print $NF}' | \
    sort | tail -n 100
UnivUtrecht_-_L-fol-802_p0-1741.png
UnivUtrecht_-_L-fol-802_p0-1743.png
UnivUtrecht_-_L-fol-802_p0-1745.png
UnivUtrecht_-_L-fol-802_p0-1747.png
UnivUtrecht_-_L-fol-802_p0-1749.png
UnivUtrecht_-_L-fol-802_p0-1751.png
UnivUtrecht_-_L-fol-802_p0-1753.png
UnivUtrecht_-_L-fol-802_p0-1755.png
UnivUtrecht_-_L-fol-802_p0-1757.png
UnivUtrecht_-_L-fol-802_p0-1759.png
UnivUtrecht_-_L-fol-802_p0-1761.png
UnivUtrecht_-_L-fol-802_p0-1763.png
UnivUtrecht_-_L-fol-802_p0-1765.png
UnivUtrecht_-_L-fol-802_p0-1767.png
UnivUtrecht_-_L-fol-802_p0-1769.png
UnivUtrecht_-_L-fol-802_p0-1771.png
UnivUtrecht_-_L-fol-802_p0-1773.png
UnivUtrecht_-_L-fol-802_p0-1775.png
UnivUtrecht_-_L-fol-802_p0-1777.png
UnivUtrecht_-_L-fol-802_p0-1779.png
UnivUtrecht_-_L-fol-802_p0-1781.png
UnivUtrecht_-_L-fol-802_p0-1783.png
UnivUtrecht_-_L-fol-802_p0-1785.png
UnivUtrecht_-_L-fol-802_p0-1787.png
UnivUtrecht_-_L-fol-802_p0-1789.png
UnivUtrecht_-_L-fol-802_p0-1791.png
UnivUtrecht_-_L-fol-802_p0-1793.png
UnivUtrecht_-_L-fol-802_p0-1795.png
UnivUtrecht_-_L-fol-802_p0-1797.png
UnivUtrecht_-_L-fol-802_p0-1799.png
UnivUtrecht_-_L-fol-802_p0-1801.png
UnivUtrecht_-_L-fol-802_p0-1803.png
UnivUtrecht_-_L-fol-802_p0-1805.png
UnivUtrecht_-_L-fol-802_p0-1807.png
UnivUtrecht_-_L-fol-802_p0-1809.png
UnivUtrecht_-_L-fol-802_p0-1811.png
UnivUtrecht_-_L-fol-802_p0-1813.png
UnivUtrecht_-_L-fol-802_p0-1815.png
UnivUtrecht_-_L-fol-802_p0-1817.png
UnivUtrecht_-_L-fol-802_p0-1819.png
UnivUtrecht_-_L-fol-802_p0-1821.png
UnivUtrecht_-_L-fol-802_p0-1823.png
UnivUtrecht_-_L-fol-802_p0-1825.png
UnivUtrecht_-_L-fol-802_p0-1827.png
UnivUtrecht_-_L-fol-802_p0-1829.png
UnivUtrecht_-_L-fol-802_p0-1831.png
UnivUtrecht_-_L-fol-802_p0-1833.png
UnivUtrecht_-_L-fol-802_p0-1835.png
UnivUtrecht_-_L-fol-802_p0-1837.png
UnivUtrecht_-_L-fol-802_p0-1839.png
UnivUtrecht_-_L-fol-802_p0-1841.png
UnivUtrecht_-_L-fol-802_p0-1843.png
UnivUtrecht_-_L-fol-802_p0-1845.png
UnivUtrecht_-_L-fol-802_p0-1847.png
UnivUtrecht_-_L-fol-802_p0-1849.png
UnivUtrecht_-_L-fol-802_p0-1851.png
UnivUtrecht_-_L-fol-802_p0-1853.png
UnivUtrecht_-_L-fol-802_p0-1855.png
UnivUtrecht_-_L-fol-802_p0-1857.png
UnivUtrecht_-_L-fol-802_p0-1859.png
UnivUtrecht_-_L-fol-802_p0-1861.png
UnivUtrecht_-_L-fol-802_p0-1863.png
UnivUtrecht_-_L-fol-802_p0-1865.png
UnivUtrecht_-_L-fol-802_p0-1867.png
UnivUtrecht_-_L-fol-802_p0-1869.png
UnivUtrecht_-_L-fol-802_p0-1871.png
UnivUtrecht_-_L-fol-802_p0-1873.png
UnivUtrecht_-_L-fol-802_p0-1875.png
UnivUtrecht_-_L-fol-802_p0-1877.png
UnivUtrecht_-_L-fol-802_p0-1879.png
UnivUtrecht_-_L-fol-802_p0-1881.png
UnivUtrecht_-_L-fol-802_p0-1883.png
UnivUtrecht_-_L-fol-802_p0-1885.png
UnivUtrecht_-_L-fol-802_p0-1887.png
UnivUtrecht_-_L-fol-802_p0-949.png
UnivUtrecht_-_L-fol-802_p0-951.png
UnivUtrecht_-_L-fol-802_p0-953.png
UnivUtrecht_-_L-fol-802_p0-955.png
UnivUtrecht_-_L-fol-802_p0-957.png
UnivUtrecht_-_L-fol-802_p0-959.png
UnivUtrecht_-_L-fol-802_p0-961.png
UnivUtrecht_-_L-fol-802_p0-963.png
UnivUtrecht_-_L-fol-802_p0-965.png
UnivUtrecht_-_L-fol-802_p0-967.png
UnivUtrecht_-_L-fol-802_p0-969.png
UnivUtrecht_-_L-fol-802_p0-971.png
UnivUtrecht_-_L-fol-802_p0-973.png
UnivUtrecht_-_L-fol-802_p0-975.png
UnivUtrecht_-_L-fol-802_p0-977.png
UnivUtrecht_-_L-fol-802_p0-979.png
UnivUtrecht_-_L-fol-802_p0-981.png
UnivUtrecht_-_L-fol-802_p0-983.png
UnivUtrecht_-_L-fol-802_p0-985.png
UnivUtrecht_-_L-fol-802_p0-987.png
UnivUtrecht_-_L-fol-802_p0-989.png
UnivUtrecht_-_L-fol-802_p0-991.png
UnivUtrecht_-_L-fol-802_p0-993.png
UnivUtrecht_-_L-fol-802_p0-995.png
UnivUtrecht_-_L-fol-802_p0-997.png
UnivUtrecht_-_L-fol-802_p0-999.png

$ # Problem with 900

##  CHANGE GREP BASED ON THE OUTPUT OF THE p0-\([0-9]\) findings
cat >/dev/null <<'EOF'

$ find . -type f -iname "*.png" | \
   awk -F'/' '{print $NF}' | \
     grep "p0[-][2-8]" | \
       wc -l

find . -type f -iname "*.png" | \
   awk -F'/' '{print $NF}' | \
     grep "p0[-][2-8]" | \
       wc -l

find . -type f -iname "*.png" | \
   awk -F'/' '{print $NF}' | \
     grep "p0[-][6-8]" | \
       wc -l

find . -type f -iname "*.png" | \
   awk -F'/' '{print $NF}' | \
     grep "p0[-]6" | \
       wc -l

find . -type f -iname "*.png" | \
   awk -F'/' '{print $NF}' | \
     grep "p0[-]7" | \
       wc -l

find . -type f -iname "*.png" | \
   awk -F'/' '{print $NF}' | \
     grep "p0[-]8" | \
       wc -l


#  DON'T NEED CHANGE


find . -type f -iname "*.png" | \
   awk -F'/' '{print $NF}' | \
     grep "p0[-]0" | \
       wc -l

find . -type f -iname "*.png" | \
   awk -F'/' '{print $NF}' | \
     grep "p0[-]1" | \
       wc -l

EOF

$ find . -type f -iname "*.png" | \
    awk -F'/' '{print $NF}' | \
      grep "_p0[-]9" | wc -l
26

$ #  Remember to change the  grep  below as needed
$ cat >/dev/null <<'EOF'

For my first fix,

find . -type f -iname "*.png" | \
   awk -F'/' '{print $NF}' | \
     grep "p0[-][6-8]" | \
       sort > more_to_rename_with_leading_0.lst

EOF

$ find . -type f -iname "*.png" | \
    awk -F'/' '{print $NF}' | \
      grep "_p0[-]9" | \
        sort > to_rename_with_leading_0.lst

$ cat to_rename_with_leading_0.lst
UnivUtrecht_-_L-fol-802_p0-949.png
UnivUtrecht_-_L-fol-802_p0-951.png
UnivUtrecht_-_L-fol-802_p0-953.png
UnivUtrecht_-_L-fol-802_p0-955.png
UnivUtrecht_-_L-fol-802_p0-957.png
UnivUtrecht_-_L-fol-802_p0-959.png
UnivUtrecht_-_L-fol-802_p0-961.png
UnivUtrecht_-_L-fol-802_p0-963.png
UnivUtrecht_-_L-fol-802_p0-965.png
UnivUtrecht_-_L-fol-802_p0-967.png
UnivUtrecht_-_L-fol-802_p0-969.png
UnivUtrecht_-_L-fol-802_p0-971.png
UnivUtrecht_-_L-fol-802_p0-973.png
UnivUtrecht_-_L-fol-802_p0-975.png
UnivUtrecht_-_L-fol-802_p0-977.png
UnivUtrecht_-_L-fol-802_p0-979.png
UnivUtrecht_-_L-fol-802_p0-981.png
UnivUtrecht_-_L-fol-802_p0-983.png
UnivUtrecht_-_L-fol-802_p0-985.png
UnivUtrecht_-_L-fol-802_p0-987.png
UnivUtrecht_-_L-fol-802_p0-989.png
UnivUtrecht_-_L-fol-802_p0-991.png
UnivUtrecht_-_L-fol-802_p0-993.png
UnivUtrecht_-_L-fol-802_p0-995.png
UnivUtrecht_-_L-fol-802_p0-997.png
UnivUtrecht_-_L-fol-802_p0-999.png

$ #
#  Your  sed  commands will almost certainly need to change.
#+ Re-run the command
$ find . -type f -iname "*.png" | \
    grep -o "_p0-[0-9]" | \
      sort | uniq -c | sort -rn
#  If your output were
#
# ``` 
#    134 _p0-1
#     50 _p0-9
#     50 _p0-8
#     50 _p0-7
#     31 _p0-6
# ```
#
#+ your  sed  part would change to
#+    sed 's#_p0-\([6-9]\)#_p0-0\1#g;'


$ while read -r line; do \
    orig="${line}"; \
    echo; \
    echo "  Moving"; \
    echo "${orig}"; \
    new_fname=$(echo "${orig}" | \
                   sed 's#_p0-9#_p0-09#g;'); \
    echo "  TO"; \
    echo "${new_fname}"; \
done < to_rename_with_leading_0.lst | head

  Moving
UnivUtrecht_-_L-fol-802_p0-949.png
  TO
UnivUtrecht_-_L-fol-802_p0-0949.png

  Moving
UnivUtrecht_-_L-fol-802_p0-951.png
  TO
UnivUtrecht_-_L-fol-802_p0-0951.png

$ while read -r line; do \
    orig="${line}"; \
    echo; \
    echo "  Moving"; \
    echo "${orig}"; \
    new_fname=$(echo "${orig}" | \
                   sed 's#_p0-9#_p0-09#g;'); \
    echo "  TO"; \
    echo "${new_fname}"; \
    echo "      ..."; \
    echo "-Command- mv \"${orig}\" \"${new_fname}\"" \
       && echo "          ... success" \
       || echo "          ... FAILURE"; \
done < to_rename_with_leading_0.lst | head

  Moving
UnivUtrecht_-_L-fol-802_p0-949.png
  TO
UnivUtrecht_-_L-fol-802_p0-0949.png
      ...
-Command- mv "UnivUtrecht_-_L-fol-802_p0-949.png" "UnivUtrecht_-_L-fol-802_p0-0949.png"
          ... success

  Moving

$ cat >/dev/null <<'EOF'

while read -r line; do\
    orig="${line}"; \
    echo; \
    echo "  Moving"; \
    echo "${orig}"; \
    new_fname=$(echo "${orig}" | \
                   sed 's#_p0-\([6-8]\)#_p0-0\1#g;'); \
    echo "  TO"; \
    echo "${new_fname}"; \
    echo "      ..."; \
    echo "-Command- mv \"${orig}\" \"${new_fname}\"" \
       && echo "          ... success" \
       || echo "          ... FAILURE"; \
done < more_to_rename_with_leading_0.lst | head -n 20

EOF


$ while read -r line; \
    do orig="${line}"; \
    echo; \
    echo "  Moving"; \
    echo "${orig}"; \
    new_fname=$(echo "${orig}" | \
                   sed 's#_p0-9#_p0-09#g;'); \
    echo "  TO"; \
    echo "${new_fname}"; \
    echo "      ..."; \
    echo "-Command- mv \"${orig}\" \"${new_fname}\"" \
       && echo "          ... success" \
       || echo "          ... FAILURE"; \
done < to_rename_with_leading_0.lst | head -n 20

  Moving
UnivUtrecht_-_L-fol-802_p0-949.png
  TO
UnivUtrecht_-_L-fol-802_p0-0949.png
      ...
-Command- mv "UnivUtrecht_-_L-fol-802_p0-949.png" "UnivUtrecht_-_L-fol-802_p0-0949.png"
          ... success

  Moving
UnivUtrecht_-_L-fol-802_p0-951.png
  TO
UnivUtrecht_-_L-fol-802_p0-0951.png
      ...
-Command- mv "UnivUtrecht_-_L-fol-802_p0-951.png" "UnivUtrecht_-_L-fol-802_p0-0951.png"
          ... success

  Moving
UnivUtrecht_-_L-fol-802_p0-953.png
  TO

$ cat >/dev/null <<'EOF'

while read -r line; do \
    orig="${line}"; \
    echo; \
    echo "  Moving"; \
    echo "${orig}"; \
    new_fname=$(echo "${orig}" | \
                   sed 's#_p0-\([6-8]\)#_p0-0\1#g;'); \
    echo "  TO"; \
    echo "${new_fname}"; \
    echo "      ..."; \
    mv "${orig}" "${new_fname}" \
       && echo "          ... success" \
       || echo "          ... FAILURE"; \
done < more_to_rename_with_leading_0.lst 2>&1 | \
            tee \
  fixing_more_awkward_order_in_1000_plus_length_$(ttdate).out

EOF

$ while read -r line; do \
    orig="${line}"; \
    echo; \
    echo "  Moving"; \
    echo "${orig}"; \
    new_fname=$(echo "${orig}" | \
                   sed 's#_p0-9#_p0-09#g;'); \
    echo "  TO"; \
    echo "${new_fname}"; \
    echo "      ..."; \
    mv "${orig}" "${new_fname}" \
           && echo "          ... success" \
           || echo "          ... FAILURE"; \
done < to_rename_with_leading_0.lst 2>&1 | \
            tee fixing_awkward_order_in_1000_plus_length_$(ttdate).out

  Moving
UnivUtrecht_-_L-fol-802_p0-949.png
  TO
UnivUtrecht_-_L-fol-802_p0-0949.png
      ...
          ... success

  Moving
UnivUtrecht_-_L-fol-802_p0-951.png
  TO
UnivUtrecht_-_L-fol-802_p0-0951.png

## ...

### ... LOTS OF OUTPUT ...

## ...

UnivUtrecht_-_L-fol-802_p0-997.png
  TO
UnivUtrecht_-_L-fol-802_p0-0997.png
      ...
          ... success

  Moving
UnivUtrecht_-_L-fol-802_p0-999.png
  TO
UnivUtrecht_-_L-fol-802_p0-0999.png
      ...
          ... success

$ grep -i --failure to_rename_with_leading_0.lst | wc -;
grep: unknown option -- failure
Usage: grep [OPTION]... PATTERNS [FILE]...
Try 'grep --help' for more information.
      0       0       0 -

$ #  Failure is not an option!

$ grep -i failure \
fixing_awkward_order_in_1000_plus_length_1751317288_2025-06-30T150128-0600.out\
    | wc -l
0

$ grep -i success \
fixing_awkward_order_in_1000_plus_length_1751317288_2025-06-30T150128-0600.out\
    | wc -l
26

$ #  Right on!

$ checksituation

 Current date/time is
Mon Jun 30 15:14:54 MDT 2025
1751318094
1751318094_2025-06-30T151454-0600

 Current directory ( pwd ) is
/cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF_-_extra_01


$





################################################
################################################
################################################
################################################
########
## OTHER TRIES USED TO PREPARE THINGS
##


################################################################
##   UNLESS YOU ARE DAVE, YOU MOST LIKELY
## DO NOT
##   WANT TO LOOK AT WHAT'S BELOW
################################################################

## Better than first try, which first try is below

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ echo >/dev/null <<EOF
> conda activate find-binding-and-unwind
> python unwind_the_binding.py
> # then watch the converted_from_PDF
> By the way, the next PDF shouldn't go into this folder, but into
> ../new_to_convert_PDF
> EOF

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ checksituation

 Current date/time is
Mon Jun 16 11:43:12 MDT 2025
1750095792
1750095792_2025-06-16T114312-0600

 Current directory ( pwd ) is
/cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF


bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ cat >/dev/null <<'EOF'
> (find-binding-and-unwind) C:\David\FHTW-2025-All_-_move_2024_get_new\Tools>python unwind_the_binding.py
pages:   0%|                                                                                   | 0/703 [00:00<?, ?it/s]
page_images:   2%|█                                                                   | 11/703 [00:03<03:58,  2.90it/s]
>
> Wait for that in Anaconda Prompt (CMD), when it gets to 703/703, copy/paste
> then [Ctrl]+[c]
> EOF

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ #  Oops, got sidetracked.

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ cat >/dev/null <<'EOF'
> (find-binding-and-unwind) C:\David\FHTW-2025-All_-_move_2024_get_new\Tools>python unwind_the_binding.py
page_images: 100%|███████████████████████████████████████████████████████████████████| 703/703 [04:21<00:00,  2.69it/s]
page_images:  53%|███████████████████████████████████▊                               | 376/703 [02:34<02:14,  2.43it/s]
pages:   0%|                                                                       | 1/703 [06:57<81:23:16, 417.37s/it]
Traceback (most recent call last):
  File "C:\David\FHTW-2025-All_-_move_2024_get_new\Tools\unwind_the_binding.py", line 46, in <module>
    pix.save(os.path.join(outdir,
  File "C:\Users\bballdave025\.conda\envs\find-binding-and-unwind\Lib\site-packages\fitz\__init__.py", line 10026, in save
    return self._writeIMG(filename, idx, jpg_quality)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\bballdave025\.conda\envs\find-binding-and-unwind\Lib\site-packages\fitz\__init__.py", line 9733, in _writeIMG
    if   format_ == 1:  mupdf.fz_save_pixmap_as_png(pm, filename)
                        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\bballdave025\.conda\envs\find-binding-and-unwind\Lib\site-packages\fitz\mupdf.py", line 43063, in fz_save_pixmap_as_png
    return _mupdf.fz_save_pixmap_as_png(pixmap, filename)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
KeyboardInterrupt
^C
(find-binding-and-unwind) C:\David\FHTW-2025-All_-_move_2024_get_new\Tools>
> EOF

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ find . -type f -iname "*.png" | grep "_p0-" | wc -l
703

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ find . -type f -iname "*.png" | grep "_p1-" | wc -l
377

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ find . -type f -iname "*.png" | grep -v "_p0-" | wc -l
377

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ find . -type f -iname "*.png" | grep -v "_p0-" | head
./UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p1-1415.png
./UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p1-1417.png
./UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p1-1419.png
./UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p1-1421.png
./UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p1-1423.png
./UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p1-1435.png
./UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p1-1437.png
./UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p1-1439.png
./UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p1-1441.png
./UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p1-1443.png

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ find . -type f -iname "*.png" | grep -v "_p0-" | tr '\n' '\0' | xargs -I'{}' -0 rm "{}"

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ find . -type f | wc -l
703

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ find . -type f -iname "*.png"| wc -l
703

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ find . -type f -iname "*.png" | sed 's#^[.]/##g;' | sort > rename_utrecht_hs3a1fol_pre.sh

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ wc -l < rename_utrecht_hs3a1fol_pre.sh
703

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ curr_idx=1; ofn="rename_utrecht_hs3a1fol.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_hs3a1fol.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" >> "${ofn}"; echo "echo \"  Moving\"" >> "${ofn}"; echo "echo \"${orig_fname}\"" >> "${ofn}"; echo "echo \"  TO\"" >> "${ofn}"; echo "echo \"${new_fname}\"" >> "${ofn}"; echo "echo \"      ...\"" >> "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" >> "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_hs3a1fol_pre.sh

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ echo -e "\x23\x21/usr/bin/env bash\n\n" > put_it_before

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ cat put_it_before rename_utrecht_hs3a1fol.sh > tmpf && mv tmpf rename_utrecht_hs3a1fol.sh

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ head rename_utrecht_hs3a1fol
head: cannot open 'rename_utrecht_hs3a1fol' for reading: No such file or directory

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ head rename_utrecht_hs3a1fol.sh
#!/usr/bin/env bash


echo
echo "  Moving"
echo "UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p0-1415.png"
echo "  TO"
echo "UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_00001.png"
echo "      ..."
mv "UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p0-1415.png" "UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_00001.png" && echo "          ... success" || echo "          ... FAILURE"

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ tail rename_utrecht_hs3a1fol.sh
echo "UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_00702.png"
echo "      ..."
mv "UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p0-2817.png" "UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_00702.png" && echo "          ... success" || echo "          ... FAILURE"
echo
echo "  Moving"
echo "UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p0-2819.png"
echo "  TO"
echo "UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_00703.png"
echo "      ..."
mv "UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_p0-2819.png" "UnivUtrecht_-_Al-Quran_HSS-Hs-3-A-1-fol_00703.png" && echo "          ... success" || echo "          ... FAILURE"

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ chmod a+x rename_utrecht_hs3a1fol.sh

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ rm rename_utrecht_hs3a1fol_pre.sh

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ rm put_it_before

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$ ./rename_utrecht_hs3a1fol.sh 2>&1 | tee renaming_utrecht_hs3a1fol_$(ttdate).out


### OUTPUT ###


bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/new_-_folder_for_not-yet-ready_new_imgs/converted_from_PDF
$





















## Next command for renames:

curr_idx=1; ofn="rename_utrecht_110e13rarfol.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_110e13rarfol.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" >> "${ofn}"; echo "echo \"  Moving\"" >> "${ofn}"; echo "echo \"${orig_fname}\"" >> "${ofn}"; echo "echo \"  TO\"" >> "${ofn}"; echo "echo \"${new_fname}\"" >> "${ofn}"; echo "echo \"      ...\"" >> "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" >> "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_110e13rarfol_pre.sh


## Next command for renames

curr_idx=1; ofn="rename_utrecht_hs3a1fol.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_hs3a1fol.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" >> "${ofn}"; echo "echo \"  Moving\"" >> "${ofn}"; echo "echo \"${orig_fname}\"" >> "${ofn}"; echo "echo \"  TO\"" >> "${ofn}"; echo "echo \"${new_fname}\"" >> "${ofn}"; echo "echo \"      ...\"" >> "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" >> "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_hs3a1fol_pre.sh


## Next command for renames

rename_utrecht_hs1b16_pre.sh

curr_idx=1; ofn="rename_utrecht_hs1b16.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_hs1b16.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" >> "${ofn}"; echo "echo \"  Moving\"" >> "${ofn}"; echo "echo \"${orig_fname}\"" >> "${ofn}"; echo "echo \"  TO\"" >> "${ofn}"; echo "echo \"${new_fname}\"" >> "${ofn}"; echo "echo \"      ...\"" >> "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" >> "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_hs1b16_pre.sh


## Next commmand for renames

rename_utrecht_w137fol_pre.sh

curr_idx=1; ofn="rename_utrecht_w137fol.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_w137fol.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" >> "${ofn}"; echo "echo \"  Moving\"" >> "${ofn}"; echo "echo \"${orig_fname}\"" >> "${ofn}"; echo "echo \"  TO\"" >> "${ofn}"; echo "echo \"${new_fname}\"" >> "${ofn}"; echo "echo \"      ...\"" >> "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" >> "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_w137fol_pre.sh


## Next command for renames

rename_utrecht_lfol773rdl1_pre.sh

curr_idx=1; ofn="rename_utrecht_lfol773rdl1.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_lfol773rdl1.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" >> "${ofn}"; echo "echo \"  Moving\"" >> "${ofn}"; echo "echo \"${orig_fname}\"" >> "${ofn}"; echo "echo \"  TO\"" >> "${ofn}"; echo "echo \"${new_fname}\"" >> "${ofn}"; echo "echo \"      ...\"" >> "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" >> "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_lfol773rdl1_pre.sh


## Next command for renames

curr_idx=1; ofn="rename_utrecht_eoct416dl1.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_eoct416dl1.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" | tee -a "${ofn}"; echo "echo \"  Moving\"" | tee -a "${ofn}"; echo "echo \"${orig_fname}\"" | tee -a "${ofn}"; echo "echo \"  TO\"" | tee -a "${ofn}"; echo "echo \"${new_fname}\"" | tee -a "${ofn}"; echo "echo \"      ...\"" | tee -a "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" | tee -a "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_eoct416dl1_pre.sh

curr_idx=1; ofn="rename_utrecht_eoct416dl1.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_eoct416dl1.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" >> "${ofn}"; echo "echo \"  Moving\"" >> "${ofn}"; echo "echo \"${orig_fname}\"" >> "${ofn}"; echo "echo \"  TO\"" >> "${ofn}"; echo "echo \"${new_fname}\"" >> "${ofn}"; echo "echo \"      ...\"" >> "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" >> "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_eoct416dl1_pre.sh


## Next command(s) for renames

rename_utrecht_foct39dl1.sh
rename_utrecht_foct39dl1_pre.sh

curr_idx=1; ofn="rename_utrecht_eoct416dl1.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_eoct416dl1.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" | tee -a "${ofn}"; echo "echo \"  Moving\"" | tee -a "${ofn}"; echo "echo \"${orig_fname}\"" | tee -a "${ofn}"; echo "echo \"  TO\"" | tee -a "${ofn}"; echo "echo \"${new_fname}\"" | tee -a "${ofn}"; echo "echo \"      ...\"" | tee -a "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" | tee -a "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_eoct416dl1_pre.sh

curr_idx=1; ofn="rename_utrecht_eoct416dl1.sh"; >"${ofn}"; while read -r line; do orig_fname="${line}"; ofn="rename_utrecht_eoct416dl1.sh"; new_fname_pre=$(echo "${orig_fname}" | sed 's#_p[0-9]\+[-][0-9]\+[.]png##g;'); new_fname="${new_fname_pre}_$(printf '%05d' ${curr_idx}).png"; echo "echo" >> "${ofn}"; echo "echo \"  Moving\"" >> "${ofn}"; echo "echo \"${orig_fname}\"" >> "${ofn}"; echo "echo \"  TO\"" >> "${ofn}"; echo "echo \"${new_fname}\"" >> "${ofn}"; echo "echo \"      ...\"" >> "${ofn}"; echo "mv \"${orig_fname}\" \"${new_fname}\" && echo \"          ... success\" || echo \"          ... FAILURE\"" >> "${ofn}"; curr_idx=$(echo "${curr_idx}+1" | bc); done < rename_utrecht_eoct416dl1_pre.sh
```
