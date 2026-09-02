<img src="https://raw.githubusercontent.com/loc-conformance/.github/main/assets/logo/loc-conformance-512.png" alt="" width="76">

# loc-conformance

#### Conformance testing of LOC counter implementations against their declared semantics.

Counters disagree with each other constantly, and most of those disagreements are not bugs.  
Each counter may have a different definition of what a "code", "comment" or "blank" line is.  
Maybe some counters don't even sort what they count into these 3 categories.  
*__And that is ok__*.

The implication of this fact is that there can never, and should never, exist an exhaustive and unified
conformance spec and test suite about what loc-counters should count and how they should count it
(like JavaScript's *test262*).

## What this organization does instead

**The one thing a counter does need to comply with is its own rules.** This organization exists
to test exactly that.  
It maintains three things:

- **A corpus** of small source files, each built around a combination of symbols that counters
  usually get wrong: an escaped quote in front of a comment, a comment symbol inside a string, a
  block comment that nests where the language says it does not.
- **A declaration format**, in which any counter can state how it counts, the categories it uses
  and the rules that fill them, as data anyone can read.
- **A published report** that measures every participating counter against its own declaration,
  with every verdict reproducible by anyone in seconds.

What makes this possible is that beneath the definitional disagreements there is **a floor of
plain facts**.  
Where each comment and each string begins and ends in a file is not a matter of
any counter's taste: the language itself decides it, in its written rules, and where the
language has a compiler, the compiler will prove it. The corpus
records those facts for every file, checked by hand, and a tool's expected numbers follow from
the facts plus its own declared rules. So a failing test always means *"this tool did not do what
it says it does"*, and never *"it disagrees with us"*.

Take a blank line inside a block comment. One counter looks only at *the line itself*: nothing is
written on it, so it calls it blank. Another looks at *where the line stands*: inside a comment,
so it calls it comment. **Both pass here**, each judged against its own rules; what neither may
do is answer one way in one file and the other way in another. A counter is free to declare
rules as unconventional as it likes: as long as it stays true to them, it passes every test.

A few things, though, are not a matter of definition. These rules apply to every counter, no
matter what it declares:

- the categories a counter reports must add up to the lines it counted,
- in a file holding more than one language, like an HTML page with a `<script>` block inside it,
  every line is charged to exactly one language, never two,
- and when the file itself says what language a part of it is written in, that settles it: a
  `<script lang="ts">` block is TypeScript, and no counter may report it as anything else.

## Why participate?

Because a counter gets more out of the suite than the suite asks of it:

- **A test suite nobody has to write alone.** Small files built around everything that breaks
  counters: escaped quotes, nested comments, strings holding comment symbols, lines spliced
  together by a trailing backslash. Every counter we have measured so far, without exception,
  failed cases here that its own tests never caught. One shared corpus, strengthened by everyone,
  replaces a dozen specific half-finished ones.
- **Regression protection for free.** Pin a version of the suite in your CI and record the
  answers your counter gives today: the build then breaks only when one of them changes, and a
  fix that quietly un-fixes itself two releases later gets caught the day it happens.
- **Failures that point at the cause.** Every test file is a few lines long and built around one
  thing only, so a failure does not tell you that some huge file comes out wrong *somewhere*, it
  tells you "you miscount an escaped quote before a comment", with the exact expected numbers
  under your own rules and the command that reproduces it. You see the bug before you have even
  opened the file.
- **Your definition, protected in writing.** Your declared rules are public documentation of what
  your counter means to do. The next time someone files "why does your tool disagree with that
  other tool", the answer is a link: both tools do what they say, and they say different things.

## What is here

[`linejudge`](https://github.com/loc-conformance/linejudge) holds the corpus of cases and the
harness that runs a counter over them.

## Status

**Not yet a stable 1.0.0.** The results page is at https://loc-conformance.github.io/linejudge/.
`linejudge` is on crates.io, and its [releases](https://github.com/loc-conformance/linejudge/releases)
carry a binary for Windows, Linux and macOS. The corpus, the declaration format and the rules are
open to change from what other counter authors find, which is what stands between this and a 1.0.

## Neutrality

[`mezura`](https://github.com/subamanis/mezura) is one of the counters measured here, and it is
written by the person who started this. Its failures are published beside everyone else's, its
declared rules sit under the same review as everyone else's, and its expected numbers come out of
the same arithmetic. The table in the result page is in alphabetical order and has every natively supported counter. What
keeps mezura's row honest is what keeps every row honest: any verdict here can be reproduced by
anyone in seconds. *The day any of this stops being true, the rest of this page is worthless.*
