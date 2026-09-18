# pinecall/docs

[docs.pinecall.io](https://docs.pinecall.io) — Mintlify, and **no prose of its own**.

Every page here is a `.md` that lives beside the code it describes, in one of the three repos.
`scripts/sync` copies them in and rewrites what only made sense inside a repo. That is the whole
design: a docs site that holds its own copy of the prose is a site that describes what the tree no
longer does, a week later, and somebody trusts it.

```bash
scripts/sync        # 43 pages and 38 images, from ../runtime, ../agents and ../protocol
mint dev            # the site, at http://localhost:3000
mint validate       # every page compiles — CI runs this
```

## What the sync does

A `.md` written for GitHub is not an `.mdx` Mintlify builds. These are the differences it closes,
each one found by a page that would not build:

- **The H1 goes.** Mintlify draws the title from the frontmatter; a second one reads as a repeat.
- **Links move.** `[the-cli.md](the-cli.md)` becomes `/operate/the-cli`. A file this site does not
  publish becomes a link to that file on GitHub, so no link dies.
- **`<picture>` becomes a `<Frame>`.** A `prefers-color-scheme` source follows the *operating
  system*, which is right on GitHub and wrong here: this site has its own light/dark toggle, and an
  image that ignores it shows a white screenshot on a dark page.
- **Prose is made safe for MDX.** `<slug>` in a usage line, `map<string, string>` in a field list,
  `{ return !!this.patient }` in a getter — MDX reads each as JSX or as an expression. Code fences
  and inline spans are found exactly (a fence pairs by its own length; a span may run over a line
  break and may open with any number of backticks) and never touched; everything else is escaped.
- **A `|` inside a code span is escaped.** GFM cuts table cells before it parses inline, so
  `` `Stages<"a"|"b">` `` splits the cell it is in.

## Adding a page

One entry in `PAGES` in `scripts/sync`: the source, where it lands, its title and its one-line
blurb. Then the same path in `docs.json`. The blurb is the only sentence this repo writes, and it
is what a reader sees under the title and in search.
