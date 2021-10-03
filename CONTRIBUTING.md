# Contributing

First of all, read how to deploy a local copy of Pan Docs in the [README](https://github.com/gbdev/pandocs#contributing).

## Git

- Commit messages must be meaningful, avoid "Update XXX.md". Please describe what you changed, how, and **why**.
- If it is desirable to squash a PR, but not into a single commit, keep in mind that most of the time, maintainers have push access to branches being PR'd ("Allow edits and access to secrets by maintainers" checkbox in the right sidebar).
- Maintainers: rebase or squash PRs, please do not create merge commits. Avoid leaving formatting/typos/etc. commits in `master`'s history.

## [Board](https://github.com/gbdev/pandocs/projects/1)

Some notes on how we use the Project board:

**Triage**: the issues here are still in the RFC phase. The discussion is on-going or not enough information is available to progress. You're welcome to join the discussion, provide references and propose solutions.

**To Do**: stuff here is ready to be worked on. You should have all the information you have to work on a PR to implement the solution required by the issues.

## Document Style

### 1. Conventions documentation

A short paragraph introducing and clarifying the conventions used in the document must be placed (and kept updated) in the top of the document.

### 2. Monospaced text, code snippets, code boxes

- No ligatures

### 3. Pseudocode

- Assignment: :=
- Comparison: =, ≠, <, >, ≤, ≥
- Arithmetic: +, −, ×, /, mod
- Floor/ceiling: ⌊, ⌋, ⌈, ⌉.     a :=    ⌊b⌋    + ⌈c⌉
- Logical: and, or, not
- Sums, products: Σ Π

References:
1. http://www.cs.cornell.edu/courses/cs482/2003su/handouts/pseudocode.pdf
2. https://blog.usejournal.com/how-to-write-pseudocode-a-beginners-guide-29956242698
3. https://cs.wmich.edu/gupta/teaching/cs3310/sp18cs3310web/lecture%20notes%20cs3310/PseudocodeBasics.pdf
4. https://en.wikipedia.org/wiki/Pseudocode#Mathematical_style_pseudocode

### 4. Units and prefixes:

Use Binary Prefixes (Ki, Mi) for Bytes. E.g.:

```
1 MiB = 2^20 bytes = 1,048,576 bytes
1 KiB = 2^10 bytes = 1,024 bytes
```

- Abbreviate bytes as "B" when used with a prefix. Spell out bits as "bits", even when used with a prefix, to avoid ambiguity. When used as a unit suffix, unit names are always singular, so it should be "20 kbit", not "20 kbits". (Also, "bytes" as a word is fine, just like "30 meters" is a valid way of talking about that length. But "Mbyte" and the like should go away.)
- Hexadecimal values are uppercase and prefixed with `$`: e.g. `$ABCD`. To prevent clutter, don't use a prefix for hex numbers when it's clear from the context that a number is hexadecimal. For example, addresses and lists of opcodes. In those cases, zero-pad, even for smaller numbers: `0000-3FFF` instead of `0-3FFF`.
- Put a space between numbers and their unit (ISO).
- Decimal numbers must be written with a decimal point instead of a comma.
- Binary numbers must be accompanied by the "binary" word.
- Other units (such as Hertz) may use the decimal prefixes (K = 1000, M = 1000000)

References:
- https://superuser.com/questions/938234/size-of-files-in-windows-os-its-kb-or-kb#comment1275467_938259

Discussion:
- [#76](https://github.com/gbdev/pandocs/issues/76), [#55](https://github.com/gbdev/pandocs/issues/55)


### 5. 8 bits / 8-bit

- "8 bits" and "8-bit" have different usages in the English language. The former is used when talking about the quantity ("a byte has 8 bits"), while the latter is used as an adjective ("8-bit bytes are nowadays standard"). "8bit" is obviously wrong, and "8 bit" is likewise incorrect.

Discussion:
- [#102](https://github.com/gbdev/pandocs/issues/102)

### 6. Horizontal Blanking Interval, Vertical Blanking Interval

- VBlank, HBlank

Discussion:
- [#94](https://github.com/gbdev/pandocs/issues/94)

### 7. Mentioning models

We use the console codenames, so:

- DMG
- SGB
- MGB (Game Boy Pocket)
- SGB2
- CGB (Game Boy Color)
- AGB

### 8. Spacing and hyphenation of some terms

- "tile map", not "tilemap"
- "wavelength", not "wave length" ([#350 (comment)](https://github.com/gbdev/pandocs/pull/350#discussion_r709713631))

### 9. Numerical ranges

Those should either be written out (1 to 42), or use an "n-dash" `–` (can also be written as the HTML entity `&ndash;`).

Discussion:
- [#341 (comment)](https://github.com/gbdev/pandocs/pull/341#discussion_r708681099)


## SVG 

### Rationale

The preferred format for diagrams in Pan Docs is SVG.
- ASCII, Unicode, or text-only diagrams are not accessible (screen readers, etc.)
- "Raster" images do not scale, and e.g. text within them cannot be selected.
- Additionally, raster images may be troublesome to post-edit without extra "source" files (e.g. XCF or PSD files).
- Lastly, SVGs can be dynamically styled, allowing for better integration within the page.

### Guidelines

SVGs can be created using a lot of programs: [Inkscape](https://inkscape.org), a text editor ([SVGs are just XML documents!](https://developer.mozilla.org/en-US/docs/Web/SVG)), and others.

**Be wary of "image to SVG converters"**, as these tend to just embed the raster image within a SVG document, which most of the time generates a larger file with none of the benefits expected from a SVG.

"Raster" images, like PNG, JPEG, BMP, and so on, store pixels.
SVGs (and more generally, "vector" graphics) instead contain commands to draw shapes.
For this reason, and because computers are bad at recognizing shapes, **converting from the former to the latter has to be largely done by hand from scratch**.
(On the other hand, it's a good time to go over any improvements that can be brought to the diagram!)

### Dynamically-styled SVGs

A good reference is [this commit](https://github.com/gbdev/pandocs/commit/be820673f71c8ca514bab4d390a3004c8273f988#diff-806f4be9ec1ba99e972f48151ca8a3f19d3048a7dbe6f39853dff85ee01806e6).
(NB: only the new SVG file and the modification to `src/pixel_fifo.md` need to be considered. The rest is cleanup and plumbing.)
It may be a good idea to check out that example if you aren't sure and want to check something.

SVGs can be displayed in a page in two ways: either linked to from an `<img>` tag, or embedded directly in the HTML document via a `<svg>` tag.
[Due to technicalities](https://stackoverflow.com/questions/18434094/how-to-style-svg-with-external-css), SVGs cannot access the parent document's style sheet when displayed using the former method.

The way themes change styling is using [CSS variables](https://developer.mozilla.org/en-US/docs/Web/CSS/--*), specifically [all of these](https://github.com/rust-lang/mdBook/blob/d22299d9985eafc87e6103974700a3eb8e24d73d/src/theme/css/variables.css#L11-L210).
The variables can be reused within a SVG's stylesheet to make it automatically update its style when a new theme is selected, but this requires embedding the SVG within the page for it to have access to those variables.

#### How-to

In theory, pasting the SVG directly within the Markdown document would work.
However, this increases its size a lot, and thus hinders navigation; it also makes the SVG impossible to view and edit on its own.
For these reasons, use of mdBook's ["include" mechanism](https://rust-lang.github.io/mdBook/format/mdbook.html#including-files) is preferred.

First, create the SVG file itself; see further above for general directions.
To have the SVG be dynamically styled, you will likely have to open it in a text editor and manually change it.
Locate the `<defs>` element directly within the `<svg>` tag, and then the `<style>` tag within that.
Create those as necessary if they aren't present.

Now, add CSS rules to style the elements (although CSS within SVGs affects [different attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute), so [some attributes have different names](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/fill#text)).
You can add classes and/or IDs to the elements as normal, and use those in CSS selectors.

Once that is done, you must ensure that the file contains **no empty lines** (whitespace doesn't count) after the opening `<svg>` tag.
Any blank lines will break the Markdown markup, and likely yield a broken image.
Additionally, remember which line the opening `<svg>` tag is on for the next step.

Then, add a directive to embed the file's contents, for example `{{#include imgs/ppu_modes_timing.svg:2:}}`.
Replace `imgs/ppu_modes_timing.svg` with the path to the SVG from the `src/` folder, and `2` with the line number the opening `<svg>` tag is on.

A word of caution about the directive's placement: if it is surrounded by empty lines, the image will be outside of any paragraphs, within the document's main flow.
However, if it is adjacent to a line of text, it may be placed within the paragraph, which is likely not what you want.