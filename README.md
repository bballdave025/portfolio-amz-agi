# portfolio-amz-agi
Portfolio set up to go along with application for Amazon AI Content Expert II, Job ID 3045592. Made to showcase how my skills align with the job description.

Want to see some of the images that go with some of the projects? Have a look at my [general portfolio](https://github.com/bballdave025/portfolio-resume),
then come back to look at what's here!

## Ready for a Rebuild (Like those old, Under Construction, banners that used to be on some websites). ETA, 2025-09-10 at 08:00:00 MDT

## Regex + UNIX test tooling

Have a look at [this regex (and UNIX) showcase](https://github.com/bballdave025/portfolio-amz-agi/blob/main/isolated_examples/regex_showcase.md)

## Familiarity with Python CLI

Ever wondered which elements can be spelled with the element symbols? Look at the `README` for [`mendeleev-spelling-bee`](https://github.com/bballdave025/mendeleev-spelling-bee).

## LLM Familiarity

Have a look at [my notes](https://github.com/bballdave025/portfolio-amz-agi/blob/main/isolated_examples/notes_on_incremental_CoT.md) about doing some incremental CoT prompting to build a Cyrillic-based chemical-element list with one-to-one mapping, cultural sensitivity, and a lot of fun.

Have a look at [my work](https://github.com/bballdave025/rwkv-lora) LoRA in anticipation of a proof-of-concept use with RWKV. RWKV is a RNN-based LLM that has shown promise in providing transormer-LLM-like results.

## JSON/CSV/Markdown/HTML/XML fluency

Look at the [same file](https://github.com/bballdave025/portfolio-amz-agi/blob/main/isolated_examples/regex_showcase.md) showcased in Regex + Unix. 
I had to get creative to get the colors working right. (For a previous attempt at color with a similar sequence of `bash` commands,
have a look [here](https://github.com/bballdave025/fhtw-paper-code-prep/blob/main/dataset_preparation_examples/steps_rename_utrecht.md).)

<hr/>

# Showcased Projects

## Reused Manuscript Fragments in Bindings

See the `README` at the [repo root](https://github.com/bballdave025/fhtw-paper-code-prep), or check out the research, code, and presentation at the 2024 Family History Technology Workshop 2024 at the [repo](https://github.com/bballdave025/manuscript-waste-reuse-finder) for that study.

## English: Germanic or Romance

Currently being worked out in a 30-minute coding session with Copilot.

## Mendeleev Spelling Bee

See the `README` at the [directory root]. Feel free to try it out. The Russian version grew out of the original project ideas partnered with practicing CoT prompting ideas.

```bash
git clone <ssh-or-html-path-from the green "Code" button> mendeleev-spelling bee.
cd mendeleev-spelling-bee
#  In setup.py, uncomment the line,
#  `#__version__ = "0.1.1"`
#  and comment out the line
#  `from mendeleevspellingbee import __version__`
#
#  I am working on a better build.
pip install .
#   Use `pip install -e .` if you want to play with the code
mendeleevspellingbee -d russian.txt -s cyrillic
```

Have a look through other repos while you're at it!
