# Resampling statistics, third edition

Material for updated (third) edition of [Resampling: The New Statistics,
second edition](http://www.resample.com/intro-text-online) by Julian L. Simon.

The new edition is by Julian L. Simon with Matthew Brett, Stéfan van der Walt
and Ian Nimmo-Smith.

The latest version will always be at the [book
website](https://resampling-stats.github.io/resampling-with).

We release the material in this repository under
a [CC-BY-ND](https://creativecommons.org/licenses/by-nd/4.0) license, unless
otherwise specified. See `LICENSE.md` in this directory for details.

The source text that we build to the book is in the `source` directory.

There are source (Markdown) versions of chapters from the second edition in the
`unported` directory.  As we fill out the third edition, we move these files
into the `source` directory, and edit them there.

## Setup for editing and proofing

```{bash}
export PIP_INSTALL_CMD="pip install"
make build-init
```

Make sure that your `rmarkdown` package is sufficiently up to date to work
with your `pandoc` version.  Versions of `pandoc` >= 2.11 use `--citeproc` and
not `--filter pandoc-citeproc`; if your `rmarkdown` version is older than 2.5
(`library(rmarkdown); sessionInfo()`), it won't know that, and therefore will
raise an error on book build - see [RMarkdown release
notes](https://github.com/rstudio/rmarkdown/releases).  Upgrade with
`install.packages('rmarkdown')`.

### Quarto

We use [Quarto](https://quarto.org) as the build machinery for the website and for PDF.

See [the Quarto installation instructions](https://quarto.org/docs/getting-started/installation.html).  Afterwards, install the matching R package.

```
Rscript -e 'install.packages("quarto")'
```

If it complains about the CRAN mirror not set, add the following to `~/.Rprofile` and try again:

```
local({r <- getOption("repos")
       r["CRAN"] <- "http://cran.r-project.org"
       options(repos=r)})
```

The process may fail if it cannot find curl and openssl development headers.
The error message explains how to install those headers on the various systems.
For example, on Fedora it'd be:

```
sudo dnf install libcurl-devel openssl-devel
```

Finally, check the installation:

```
quarto check install
quarto check knitr
```

Quarto uses various [Pandoc markdown
extensions](https://linux.die.net/man/5/pandoc_markdown), as do we (Div and
Span elements for custom inline elements and blocks).

## Building the book

See `source/README.md` and `notes/writing.md`.

# Writing and updating the text

First follow the build instructions above.

Activate your virtual environment for the book, if you are using one.  In my
(MB's) case, I use [Virtualenvs](https://virtualenv.pypa.io) and
[VirtualenvWrapper](https://virtualenvwrapper.readthedocs.io) to do this:

```
workon resampling-with
```

This activates my installed virtualenv environment for the book.


Make sure you can build the whole book in your current environment with:

```
make clean && make python-book
make clean && make r-book
```

from the top-level repository directory.   If this doesn't work, make an [Issue
on Github](https://github.com/resampling-stats/resampling-with/issues).

After you've confirmed you can build both the Python and the R edition, you may
want to work on only one of these editions — say the Python book, and clean up
the R book later (or the other way round).

Matthew and Peter know R reasonably well — we can help with R cleanup.

## Starting work on a new chapter

See the `./source/_quarto.yml.template` file for a list of the chapters
currently in the book build.  You may see that there are commented-out chapters
from the original book; these do not get built to the online version of the
book.

Let's say you want to start work on one of the chapters, and you've see this in the `_quarto.yml.template` file:

```
#   - reliability_average.Rmd
```

The procedure is:

Before you start:

* Make a new Git branch, and check out the branch.
* `cd source`
* Do (for Python) `make clean && make python-book`.

Editing:

* Edit the matching file - here `source/reliability_average.Rmd`.
* You might find it useful to have the original PDF chapter open; see [the
original book in PDF](https://resample.com/intro-text-online).  To see which
original chapter corresponds to your current `.Rmd` file, have a look at the
YAML fragment at the file, it should have something like: `ed2_fname:
26-Chap-22`.
* Rebuild the file you're working on from time to time with e.g.
`../scripts/rebuild_chapter reliability_average.Rmd` from the `source`
directory. Check the contents by opening e.g.
`../python-book/reliability_average.Rmd` (Quarto will show the correct filename
after the rebuild step).
* When in some kind of shape that is ready for other people to look at,
uncomment the filename in `_quarto.yml.template`.
* Make your commits with the changes, and do a pull-request to the main
repository, for us to look at.

## Citations

Citations are in [Pandoc format](https://pandoc.org/MANUAL.html#citations), as
implemented in [Quarto's
citations](https://quarto.org/docs/authoring/footnotes-and-citations.html).

Check that the reference is not already in `source/simon_refs.bib`.  Add it if
so, following reference name standard in that file (e.g.
`@article{christensen2005fisher,`).  Cite with e.g. `This is a terrible idea
[@christensen2005fisher]` or `As Christensen notes [-@christensen2005fisher]`,
or `There are many good ways to do this [see @knuth1984, pp. 33-35; also
@wickham2015, chap. 1]`.  See Quarto link above for other examples.

## Footnotes

See [Quarto footnotes](https://quarto.org/docs/authoring/footnotes-and-citations.html#footnotes))

Examples (from that page):

```
Here is a footnote reference,[^1] and another.[^longnote]

[^1]: Here is the footnote.

[^longnote]: Here's one with multiple blocks.

  Subsequent paragraphs are indented to show that they
  belong to the previous footnote.

Here is an inline note.^[Inlines notes are easier to write,
since you don't have to pick an identifier and move down to
type the note.]
```

## Cross-references

See [Cross-references in
Quarto](https://quarto.org/docs/authoring/cross-references.html).  Summary for
section reference: add `{#sec-name-for-your-ref}` to the target section title,
reference with `Please see section @name-for-your-ref for details`.

## Useful links

* [The original book in PDF](https://resample.com/intro-text-online). MB has
the print book.

## Notes for concepts in other discussions in the book

See [the notes repository](https://github.com/resampling-stats/resampling-roam)
for more discussions of various concepts in the book, and how we are thinking
about them.
