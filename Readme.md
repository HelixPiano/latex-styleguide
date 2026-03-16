# LaTeX guideline for your scientific manuscript
Author: Philipp Heinrich  
ORCID: 0009-0005-8887-3141

This document contains information, tips, and guidelines that I collected while working on various LaTeX files for scientific manuscripts. It may give you some ideas on what to look out for when creating your own manuscript. The information provided here have been taken from all sorts of documents and forum posts.
I want to especially highlight Diomidis Spinellis' wonderful github repository, which has an even more extensive list [\[1\]](#1).  

Everything listed here is a suggestion. Make sure you check your journal's submission guidelines properly. Fix all LaTeX errors immediately to make life easier for your co-authors. Use the 'Stop on first error' option in Overleaf to help with this.

## Text

- Check for non-ASCII characters (e.g. Umlaute) using a regex search: `[^\x00-\x7F]`.
- Instead of using `/` in running text, use `\slash`.
- For underscores in text, use `\_`.
- Make sure your hyperlinks do not break across lines.
- After e.g., i.e., and other non-sentence ending dots insert a protected space if not followed by a comma, for example: `e.g.\ lorem ipsum!`.
- If there is a capital letter preceding a sentence-ending period you have to add an `\@` so that LaTeX does not treat it as an abbreviation. Example: `...Pentium III\@. This...`. Regex search for `[A-Z]\.`[\[5\]](#5)
- Ensure that acronyms are defined only once and before they are used. Avoid acronyms for terms that appear only once.
- For single quotation marks, use `` `text'``. American English uses double quotation marks as the default; British English uses single quotation marks, reserving double quotation marks for quotations within quotations. If the journal allows additional packages use `csquotes` to handle everything automatically for you.
- Ranges require an en dash (`--` in LaTeX) with no spaces between start and end (e.g. `1--10`, `Jan--Feb`, `1961--1990`).
- Check for double spaces.
- Check for missing or extra braces, especially in long paragraphs.
- Use `Section~\ref{sec:...}` or `Sect.~\ref{sec:...}` depending on journal.
- Check for consistent spelling (British vs. American).
- Different guidelines will give you different advice on which words should be capitalized in a header or section title. Just try to keep it consistent.
- Ellipses: Use `\dots` instead of writing `...` yourself.

## LaTeX code aesthetics

- Remove unused packages to avoid conflicts.
- Remove trailing whitespace, i.e. no line may end with a whitespace character.
- Handle LaTeX code like your software code. It should be readable and easily maintainable.
- It is easier to spot errors if you insert a line break after each sentence instead of writing very long paragraphs.
- Remove text that is no longer needed instead of commenting it out. This keeps the manuscript cleaner and easier to read.
- Put large tables in a separate tex file to avoid cluttering the code. The table can then simply be inserted via `\input{file_name}` (do not add the .tex file extension).
- Use a linter like LaCheck (`lacheck`) or ChkTeX (`chktex`) to find problems in your LaTeX code.

## Math and equations

- Use `$\pm$` instead of writing `+-`.
- A useful list of mathematical symbols:  
  https://www.cmor-faculty.rice.edu/~heinken/latex/symbols.pdf.
- Put numbers that are part of math/science and not part of the English language in math mode. Or how [\[6\]](#6) put it:
  > If it's math, put it in math mode; if it's not math, don't put it in math mode.
- For single-line display math, use `\[ ... \]` instead of `$$...$$`.
- Use `\left` and `\right` to scale brackets.
- Use native LaTeX commands like `\sin`.
- Copernicus Guidelines: "In the text, equations should be referred to by the abbreviation "Eq." and the respective number in parentheses, e.g. "Eq. (14)". However, when the reference comes at the beginning of a sentence, the unabbreviated word "Equation" should be used [...]".

## Units

- Coordinates need a degree sign and a space when naming the direction (e.g. `30$^{\circ}$~N`).
- Spaces must be included between number and unit (e.g. 1 %, 1 m) [There is no consensus on whether to include a space before the percent sign in English].
- Units must be written exponentially (e.g. W m<sup>–2</sup>).
- [Wikipedia on ranges with units](https://en.wikipedia.org/wiki/Dash#En_dash): Some style guides [...] recommend that, when a number range might be misconstrued as subtraction, the word "to" should be used instead of an en dash. For example, "a voltage of 50 V to 100 V" is preferable to using "a voltage of 50–100 V". Relatedly, in ranges that include negative numbers, "to" is used to avoid ambiguity or awkwardness (for example, "temperatures ranged from −18 °C to −34 °C").
- Common abbreviations to be applied: hour as h (not hr), kilometre as km, metre as m.
- Journals have different guidelines for units. Some have their own command; otherwise use siunitx if additional packages are allowed. There you can write 10 km h<sup>–1</sup> via `\qty{10}{\kilo\metre\per\hour}`.
- SI base units and SI‑accepted units must be abbreviated in conjunction with numbers (e.g. the velocity is 10 km h<sup>–1</sup>) and must be written out without numbers (e.g. the velocity is given in kilometres per hour).

## Supplementary material

- When linking to an external document, use a prefix to avoid label collisions, e.g.  
  `\externaldocument[sup-]{supplemental_material}`.  
  This means that in `main.tex` you must change references from `\ref{fig:x}` to `\ref{sup-fig:x}`.

- Use these commands to add an "S" to the figures, tables, and sections in the supplementary material:
  `\makeatletter`
  `\renewcommand\thesection{S\@arabic\c@section}`  
  `\renewcommand\thetable{S\@arabic\c@table}`  
  `\renewcommand\thefigure{S\@arabic\c@figure}`
  `\makeatother`

## Figures and tables

- Make sure the figures are colour-blind friendly. You can check them, for example, at https://www.color-blindness.com/coblis-color-blindness-simulator/.
- Not all journals allow tables with coloured text.
- Connect the words Fig. or Table with the reference label, e.g. `Fig.~\ref{fig01}`.
- Check how the journal handles abbreviations of Figure and Table in text. Excerpt from the Copernicus guidelines:  
  > The abbreviation "Fig." should be used when it appears in running text and should be followed by a number unless it comes at the beginning of a sentence, e.g.: "The results are depicted in Fig. 5. Figure 9 reveals that...".  
  > Please note that the word "Table" is never abbreviated and should be capitalized when followed by a number (e.g. Table 4).
- Use vector formats like PDF for figures. Otherwise PNG is preferred over JPG.
- Journals often want a multipanel figure to be submitted as a single file.
- You can use `\FloatBarrier` from the *placeins* package to force floats to stay in a certain region. If you want them to stay exactly where they appear in the code, you can force it with `[!H]` from the *float* package. Best practice is to let LaTeX decide using `[!htbp]`.
- An empty line after a float environment begins a new paragraph, so avoid this unless intended: 
    ```latex
    Text here.
    \begin{figure}[!htbp]
    \centering
    \includegraphics[width=\textwidth]{Fig_S02.pdf}
    \caption{Lorem ipsum dolor sit amet.}
    \label{FIG:Lorem_Ipsum}
    \end{figure}

    The empty line above makes this a new paragraph.
    ```
- If you want horizontal rules in tables, use `\toprule`, `\midrule`, and `\bottomrule` from the *booktabs* package.
- Books and guidelines about typography and design recommend avoiding `\hline` and `|` in tables for aesthetic reasons.
- Label the figure’s axes, make sure the font size is large enough, and avoid common chart errors: https://github.com/cxli233/FriendsDontLetFriends
- Make sure all figures and tables are referenced in the text.
- Place table captions above the table, unless specified otherwise by the journal.

## Bibliography

- Check for non‑ASCII characters via regex search: `[^\x00-\x7F]`.
- Do **not** put a link in the DOI field, only the identifier itself, for example: `doi = {10.1000/182}`.
- Check for unused citations with checkcites.lua: https://github.com/islandoftex/checkcites.
  [Some systems require placing the script directly in the working folder.]
- Beautify your BibTeX file by sorting it alphabetically, aligning fields, etc.:  
  https://flamingtempura.github.io/bibtex-tidy/
- Avoid nonstandard fields (month, publisher, address) unless required by the journal.
- Remove unused fields by checking the `.bib` file in your local command line with biber:  
  `biber --tool --validate-datamodel Bibliography.bib`.
- Use `date` instead of `year` if possible; it is newer, but not always supported by journals.
- Make sure the author list does **not** contain “et al.”, “and others”, or similar.
- Use `url` and `urldate` to record when a website was accessed, **not** the `note` field.
- When collaborating, agree on a consistent scheme for reference keys.  
  For example `<author><year><titleword>` → `heinrich2023compound`.
- Include multiple references in the same `\cite` command (e.g. `\cite{Doh19,Spi22}`) rather than using multiple separate `\cite` commands (`\cite{Doh19} \cite{Spi22}`).
- Ensure the journal name is written out in your `.bib` file; some tools abbreviate them automatically.
- Do not include a URL for entries that have a DOI, since the DOI is more authoritative and URLs may decay over time.
- Put the name of organizations, or similar, that should not be treated as regular names into extra braces `author = {{European Commission}}`.
- Make use of the `howpublished` field for `@misc` entries to address those source types that are not directly supported by BibTeX. This field comes in handy, especially when you are citing web pages and you want to provide a URL [\[4\]](#4).

## Resources

<a id="1"></a>[1] https://github.com/dspinellis/latex-advice?tab=readme-ov-file

<a id="2"></a>[2] https://github.com/cxli233/FriendsDontLetFriends

<a id="3"></a>[3] https://web.evanchen.cc/latex-style-guide.html

<a id="4"></a>[4] https://bibtex.eu/fields/howpublished/

<a id="5"></a>[5] https://john.regehr.org/latex/

<a id="6"></a>[6] https://www.economics.utoronto.ca/osborne/latex/LTXERR.HTM
