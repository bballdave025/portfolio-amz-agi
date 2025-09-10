# Notes on Incremental CoT Prompting for Cyrillic-Based Element List

This experiment explores how to use a flexible, iterative prompting strategy to build a Cyrillic-script chemical element list from multilingual sources. While not technically "pure" Chain-of-Thought (CoT) prompting, the process mirrors its spirit—breaking down reasoning into manageable steps and refining outputs through guided iteration.

## Goals
- Extract element names and a word list from Russian-language sources (e.g., Wiktionary, Hunspell, Sketch Engine)
- Map Cyrillic spellings to Latin element symbols (e.g., `Фл` → `Fl`)
- Use incremental CoT prompting to:
  - Normalize transliterations
  - Resolve ambiguous mappings
  - Filter out non-constructible forms

## Prompting Style
This isn't a single-shot CoT prompt. Instead, it's:
- **Incremental**: Each round builds on the last, refining logic and edge cases
- **Linguistically aware**: Prompts include phonetic hints, IPA, and transliteration variants
- **Symbol-sensitive**: Encourages the AI to reason about valid element symbol sequences (e.g., `Xe|No|N`)
- **Contextual**: Adjusts based on prior outputs, false positives, and semantic drift

## Design Principles for Cyrillic Symbol Mapping
- Prioritize one-letter symbols for the most common elements  
- Use Greek-derived names where they exist in Cyrillic languages  
- Create consistent rules for elements discovered by Russian/Soviet scientists  
- Consider using both uppercase and lowercase to expand options  
- Especially with the trans-fermionic elements, give priority to Soviet and Russian discoverers  

## Seeded Examples
To scaffold the prompting process, I seeded the model with **21 curated examples**—each consisting of:
- Latin element name  
- Latin element symbol  
- Russian element spelling (Cyrillic)  
- IPA pronunciation of the Russian name  
- Proposed Cyrillic symbol  
- Justification for the mapping

These examples were chosen to cover a range of phonetic mappings, symbol lengths, and edge cases. They served as grounding data for incremental prompting and symbol validation.

See the full [Cyrillic CoT Seed Table](#cyrillic-cot-seed-table) below.

## Challenges
- Cyrillic ambiguity (e.g., `И` vs `Й`)
- Symbol overlap (e.g., `С` could be `S`, `Sc`, or `Cs`)
- Maintaining chemical validity while allowing creative mappings

## Next Steps
- Build a test suite for Cyrillic-to-symbol decoding
- Compare against Latin-script element list for coverage
- Document edge cases and false positives
- Explore fuzzy matching or phonetic similarity scoring

This approach blends linguistic nuance with symbolic reasoning—perfect for showcasing AI-assisted multilingual tooling. It’s not just CoT—it’s **interactive symbolic decoding**, tuned for real-world ambiguity and creative exploration.
 
---

## Cyrillic CoT Seed Table

| Element Name | Latin Symbol | Russian Name (Cyrillic) | IPA Pronunciation | Proposed Cyrillic Symbol | Justification |
|--------------|--------------|--------------------------|--------------------|---------------------------|---------------|
| Hydrogen     | H            | Водород                  | [vədɐˈrot]         | В                         | First letter of the Russian name. |
| Helium       | He           | Гелий                    | [ˈɡʲelʲɪj]         | Ге                        | Russian name starts with a different letter than Hydrogen. |
| Lithium      | Li           | Литий                    | [ˈlʲitʲɪj]         | Ли                        | First two letters of the Russian name. |
| Beryllium    | Be           | Бериллий                 | [bʲɪˈrʲilʲɪj]      | Бе                        | First two letters of the Russian name. |
| Boron        | B            | Бор                      | [bor]              | Б                         | First letter of the Russian name. |
| Carbon       | C            | Углерод                  | [ʊɡlʲɪˈrot]        | У                         | First letter of the Russian name. |
| Nitrogen     | N            | Азот                     | [ɐˈzot]            | Аз                        | First two letters of the Russian name. Avoids conflict with Aluminium. |
| Oxygen       | O            | Кислород                 | [kʲɪslɐˈrot]       | Ки                        | First two letters of the Russian name. |
| Fluorine     | F            | Фтор                     | [ftor]             | Ф                         | First letter of the Russian name. |
| Sodium       | Na           | Натрий                   | [ˈnatrʲɪj]         | На                        | First two letters of the Russian name, matches Latin root. |
| Potassium    | K            | Калий                    | [ˈkalʲɪj]          | К                         | First letter of the Russian name, matches Latin root. |
| Iron         | Fe           | Железо                   | [ʐɨˈlʲezə]         | Же                        | First two letters of the Russian name. Phonetic shift from Ferrum. |
| Copper       | Cu           | Медь                     | [mʲedʲ]            | Ме                        | First two letters of the Russian name. |
| Silver       | Ag           | Серебро                  | [sʲɪrʲɪˈbro]       | Се                        | First two letters of the Russian name. |
| Gold         | Au           | Золото                   | [ˈzolətə]          | Зо                        | First two letters of the Russian name. |
| Mercury      | Hg           | Ртуть                    | [rtutʲ]            | Рт                        | First two letters of the Russian name. |
| Lead         | Pb           | Свинец                   | [svʲɪˈnʲet͡s]       | Св                        | First two letters of the Russian name. |
| Tungsten     | W            | Вольфрам                 | [vɐlʲˈfram]        | В                         | Matches first letter of Russian name. |
| Chlorine     | Cl           | Хлор                     | [xlor]             | Хл                        | First two letters of the Russian name. |
| Phosphorus   | P            | Фосфор                   | [ˈfosfər]          | Фо                        | First two letters of the Russian name. |
| Bismuth      | Bi           | Висмут                   | [ˈvʲismut]         | Ви                        | First two letters of the Russian name. |

These examples were used to guide the AI through Cyrillic-to-symbol reasoning, helping it learn how to match Russian spellings to valid Latin element symbols according to my principles. 
They also served as a sanity check for symbol coverage, phonetic fidelity, and cultural attribution.
