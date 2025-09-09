Okay, I have spent too much time trying to get the others all formatted to highlight stuff. I'm just hoping ChatGPT can look in here, 
figure out the basics, and pull in at least one example of: 1) Looking for non-ASCII and ASCII-but-control-character-or-space, fixing;
2) Looking for my no-no characters, those that I don't like to have in a filename because of possible parsing problems.

I'm just including the logfile/instructions for the day that I went through a lot of this stuff. This one is has input and output from the terminal.

It comes from [here](https://github.com/bballdave025/fhtw-paper-code-prep/blob/main/dataset_preparation_examples/guide_example_for_ensuring_filename_characters_are_okay.log)

```bash
bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ ##    NEXT

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ cat files_to_rectify_1753973399_2025-07-31T084959-0600.list
# '.' is '/cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/'\
#'Classified_-_Almost_Just_3lett_FS_-_Combined'
./Connecting_or_Guard_Reuse/FamilySearch_-_DGS Бг҃ъ 008268002_00568_scg.jpg
./Multiple_Binding_Reuse_Classes/Besançon Bibliothèqe Municiπale_-_color-MS¶0716_00122_mbr_ucr_orc.jpg
./Multiple_Binding_Reuse_Classes/FamilySearch="iħm xp̄m ⁊ dm̄"_-_DGS004459005_00069_mbr_mcl_fmr_cwa.jpg
./Multiple_Binding_Reuse_Classes/FamilySearch_-_DGS008279842_00131%fko_mbr_orc_ucr.jpg
./No_For_Binding_Reuse/BiblioþequesMediathequesDeMetz_-_Ms № 464_00092_0047B_oic_nbr.jpg
./No_For_Binding_Reuse/BnF-BiɓArsenal_-_Ms-87_00203_oic_nbr.jpg
./No_For_Binding_Reuse/Bodleian-Library?-MS-(AddAæ)-263°_00137_oic_nbr.jpg
./No_For_Binding_Reuse/FamilySearcℏ_-_DGS004763803_00695_oic_nbr.jpg
./No_For_Binding_Reuse/Family~Search_-_DGS005141812-ꝯ_00004_oic_nbr.jpg
./No_For_Binding_Reuse/FamilɤSearchɟ_—_DGS004321721&_00066@_oic_nbr.jpg
./No_For_Binding_Reuse/Leiden_-_DeSobrietate&BPL190_00052_nbr.jpg
./Outside_Cover_Reuse/UnivBologna-SistemaMusealed'Ateneo-AlmaMaterStudiorum_-_ErbarioAldrovandi-Vol-ᴠɪ-60001_orc.jpg
./Spine_Protection_Reuse/Heidelberg_-_'British quote marks'WappenlisteAebteSalem_salMsUnkn-1876_00001_spr.jpg
./Under_Cover_Reuse/FamilySearch_-_DGS102439140☞00014_ucr_suh_mcl.jpg

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "*æ*" | wc -l
0

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  That one has been completed

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*"
./No_For_Binding_Reuse/FamilySearcℏ_-_DGS004763803_00695_oic_nbr.jpg

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  Remember that directory.

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ ls No_For_Binding_Reuse/Family*4763803*695*
No_For_Binding_Reuse/FamilySearcℏ_-_DGS004763803_00695_oic_nbr.jpg

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  No quotes there.

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {}
./No_For_Binding_Reuse/FamilySearcℏ_-_DGS004763803_00695_oic_nbr.jpg
bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  No quotes there, but following the pattern won't hurt.

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  Any non-ASCII characters or characters that are non-printable ASCII?

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]"
▒
▒
▒

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  -^- Helpful just in knowing some are there. We  grep-ped  for one byte

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #+ and any non-ASCII are 2+ bytes (in UTF-8)

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n' | wc -c # This will tell us the number of _bytes_, not generally the number of _characters_
3

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n'
ℏ
bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  Just a tofu on my terminal.

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n' | sed 's#\(.\)#'"'"'\1'"'"', #g; s#, $##g;'
'ℏ'
bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n' | xxd
00000000: e284 8f                                  ...

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  There's only one UTF-8 encoded character this time, but let's follow the
#+ pattern.

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
$ #
#  The tofu, again, hahaha, but copy/paste inside the terminal keeps the byte content.
#+ or ...

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ type getunicode4char
getunicode4char is a function
getunicode4char ()
{
    if [ "$@" = "-h" -o "$@" = "--help" ] 2> /dev/null; then
        echo "Help for getunicode4char:";
        echo;
        echo "Get the unicode codepoint representing the";
        echo "character given as input.";
        echo;
        echo "Requires: python3 for now, till I get the hexdump -C stuff finished";
        echo "It was originally built with Python 3 in mind, but it works";
        echo "without that, thanks to hexdump -C";
        echo "You'll still need to watch out for the problem strings";
        echo "methioned below.";
        echo;
        echo "Usage:";
        echo "% getunicode4char <string-with-one-character>";
        echo;
        echo "Examples:";
        echo "% getunicode4char <string-with-one-character>";
        echo;
        echo "Note that you might have to copy/paste the glyph of the";
        echo "character into some type of programmer's notebook with the";
        echo "encoding set as you want it (I want UTF-8). Then, in the";
        echo "notebook, write in the  getunicode4char  part as well as";
        echo "the quotes around the character.";
        echo;
        echo "It will be easier (less backslash escapes) if you use single";
        echo "quotes around the character you pass in. The only exception";
        echo "(for ASCII, at least), is the single quote. For that, use:";
        echo "% getunicode4char \"'\"";
        echo "Do note, however, that to get a return for the space character,";
        echo "You must escape it with a backslash, whether you surround it";
        echo "with single quotes, double quotes, or just put it in by itself.";
        echo " GOOD)% getunicode4char '\ '";
        echo ' GOOD)% getunicode4char "\ "';
        echo "      % #  This next one needs explaining. You should push the";
        echo "      % #+ Space Bar once after the backslash and then press the";
        echo "      % #+ Enter key.";
        echo " GOOD)% getunicode4char \ ";
        echo "--BAD)% getunicode4char ' '";
        echo '--BAD)% getunicode4char " "';
        echo;
        echo "If you really want to use double quotes, watch out for the";
        echo "following, which will not allow the program to continue -";
        echo "i.e. they will break the program.";
        echo "--BAD)% getunicode4char \"\\\"";
        echo " instead, use single quotes or";
        echo " GOOD)% getunicode4char \"\\\\\"";
        echo "--BAD)% getunicode4char \"\`\"";
        echo " instead, use single quotes or";
        echo " GOOD)% getunicode4char \"\\\`\"";
        echo "--BAD)% getunicode4char \"\"\"";
        echo " instead, use single quotes or";
        echo " GOOD)% getunicode4char \"\\\"\"";
    else
        if [ "$@" = "'" ]; then
            echo "U+0022";
        else
            if [ "$@" = "\"" ]; then
                echo "U+0028";
            else
                if [ "$@" = "\\" ]; then
                    echo "U+005c";
                else
                    python3_is_installed=0;
                    command -v python3 > /dev/null 2>&1 && python3_is_installed=1;
                    if [ $python3_is_installed -eq 1 ]; then
                        zeroX_str=$(python3 -c 'print(hex(ord('"'$@'"')))');
                        hex_only_str=$(echo "${zeroX_str}" | sed 's#0x##g');
                        while [ ${#hex_only_str} -lt 4 ]; do
                            hex_only_str="0${hex_only_str}";
                        done;
                        echo "U+${hex_only_str}";
                    else
                        echo "won't work for now. need python3";
                        return -1;
                    fi;
                fi;
            fi;
        fi;
    fi;
    return 0
}

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ echo -e "\xe2\x84\x8f"
ℏ

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ getunicode4char "$(!!)"
getunicode4char "$(echo -e "\xe2\x84\x8f")"
U+210f

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  Didn't even have to copy/paste the tofu, there.

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ echo -e "\u210f"
ℏ

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  tofu

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ echo -en "\u210f" | xxd
00000000: e284 8f                                  ...

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  Anyway, <strike>in case there's not tofu,</strike> 
#+ the best display would be something that happened right under 
my first view of the tofu

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n' | sed 's#\(.\)#'"'"'\1'"'"', #g; s#, $##g;'
'ℏ'
bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  That should be done even if there's tofu.

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  Any of Dave's no-no ASCII characters?
#+ REMEMBER THE awk AND THE CHECK FOR SINGLE QUOTE/S, (')
#+ (that doesn't hint that I need a function, BTW)

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {}  # Looking for a single quote. This also gives us the classification dir
./No_For_Binding_Reuse/FamilySearcℏ_-_DGS004763803_00695_oic_nbr.jpg
bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {} | awk -F'/' '{print $NF}'
Bodleian-Library-MS-Add-A-263_00137_oic_nbr.jpg

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  -^- No (on-the-edge) single quote. If there were one, we'd just do
#+ a pipe to   sed 's#'"'"'$##g   after the  awk  and before the  grep

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {} | awk -F'/' '{print $NF}' | grep -o '[] ~!@#$%^&|\/)(}{[*?><;:"`'"'"']'

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {} | awk -F'/' '{print $NF}' | grep -o '[] ~!@#$%^&|\/)(}{[*?><;:"`'"'"']' | wc -l
0

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  To be sure there are none:

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {} | awk -F'/' '{print $NF}' | grep -o '[] ~!@#$%^&|\/)(}{[*?><;:"`'"'"']' | tr -d '\n' | wc -c  # would give us number of bytes, not generally the number of characters
0

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  Investigation of the filename validates our finding.

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  Since I want this example to be a guide, I'm going to put in a heredoc
#+ with a previous filename's processing for Dave's no-nos

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ cat >/dev/null <<'EOF'
> 
> Here are the commands for another filename, where there were problems
> in both checks (ASCII, Dave's no-no).
> 
> $ find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {} | awk -F'/' '{print $NF}' | sed 's#^#'"'"'#g'
> $ #  Note the lone single quote; you should remember the awk and the ' check
> $ #  REMEMBER THE awk AND THE CHECK FOR SINGLE QUOTE(S), (')
> 
> $ find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {} | awk -F'/' '{print $NF}' | sed 's#'"'"'$##g' | LC_ALL=C grep -o '[] ~!@#$%^&|\/)(}{[*?><;:"`'"'"']'
> ?
> (
> )
> 
> $ find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {} | awk -F'/' '{print $NF}' | sed 's#'"'"'$##g' | LC_ALL=C grep -o '[] ~!@#$%^&|\/)(}{[*?><;:"`'"'"']' | tr -d '\n' | wc -c  # gives n_bytes, not generally n_characters
> 3
> 
> $ find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {} | awk -F'/' '{print $NF}' | sed 's#'"'"'$##g' | LC_ALL=C grep -o '[] ~!@#$%^&|\/)(}{[*?><;:"`'"'"']' | tr -d '\n'
> ?()
> 
> # (There actually won't be a blank line after the `?()'
> $ #  This is the best view, the one to look at before changing the filename
> $ #+ (if that be necessary).
> 
> #  Once again, no blank line, and I should start with -v-
> #+ Wait, there will be a blank line.
> $ find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {} | awk -F'/' '{print $NF}' | sed 's#'"'"'$##g' | LC_ALL=C grep -o '[] ~!@#$%^&|\/)(}{[*?><;:"`'"'"']' | tr -d '\n' | sed 's#\(.\)#'"'"'\1'"'"', #g; s#, $##g;'
> '?', '(', ')'
> $ #
> #  Now, for both of the best views, after a view of the quoted
> #+ filename, for reference before renaming.
> 
> $ find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {}
> './No_For_Binding_Reuse/Bodleian-Library?-MS-(AddAæ)-263°_00137_oic_nbr.jpg'
> $  #  Non-ASCII and non-printable ASCII (control)
> 
> find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n' | sed 's#\(.\)#'"'"'\1'"'"', #g; s#, $##g;'
> 'æ', '°'
> $ #  Dave's no-nos
> 
> $ find . -type f -iname "Bodl*MS*Add*263*137*" -print0 | xargs -I'{}' -0 printf "%q" {} | awk -F'/' '{print $NF}' | sed 's#'"'"'$##g' | LC_ALL=C grep -o '[] ~!@#$%^&|\/)(}{[*?><;:"`'"'"']' | tr -d '\n' | sed 's#\(.\)#'"'"'\1'"'"', #g; s#, $##g;'
> '?', '(', ')'
> #  Now, we can do the rename, after checks
> 
> EOF

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  For ours, here, we only have non-ASCII

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {} | LC_ALL=C grep -oP "[\x00-\x20\x7F-\xFF]" | tr -d '\n' | sed 's#\(.\)#'"'"'\1'"'"', #g; s#, $##g;'
'ℏ'
bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  Now, we can rename.
#+  REMEMBER THE CLASSIFICATION DIRECTORY

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*" -print0 | xargs -I'{}' -0 printf "%q" {}
./No_For_Binding_Reuse/FamilySearcℏ_-_DGS004763803_00695_oic_nbr.jpg
bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*"
./No_For_Binding_Reuse/FamilySearcℏ_-_DGS004763803_00695_oic_nbr.jpg

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  Luckily, they're the same in this case.
#+ Always use the one with  printf , by the way.

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #
#  Let's see if the fixed version somehow exists,
#+ first checking if we have an example of how other images from that
#+ document are named.

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*" | head -n 5
./No_For_Binding_Reuse/FamilySearch_-_DGS004763803_00225_oic_nbr.jpg
./No_For_Binding_Reuse/FamilySearch_-_DGS004763803_00226_oic_nbr.jpg
./No_For_Binding_Reuse/FamilySearch_-_DGS004763803_00227_oic_nbr.jpg
./No_For_Binding_Reuse/FamilySearcℏ_-_DGS004763803_00695_oic_nbr.jpg

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "FamilySearch_-_DGS004763803_00695_oic_nbr.jpg" | wc -l
0

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

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  Post-process checks

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "*.jpg" | wc -l
3331

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  good

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "Family*4763803*695*"
./No_For_Binding_Reuse/FamilySearch_-_DGS004763803_00695_oic_nbr.jpg

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  good

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ find . -type f -iname "*ℏ*" | wc -l
0

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ #  good

bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$ checksituation

 Current date/time is
Sat Aug  2 08:43:56 MDT 2025
1754145836
1754145836_2025-08-02T084356-0600

 Current directory ( pwd ) is
/cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined


bballdave025@MY-MACHINE /cygdrive/c/David/FHTW-2025-All_-_move_2024_get_new/Classified_-_Almost_Just_3lett_FS_-_Combined
$
```
