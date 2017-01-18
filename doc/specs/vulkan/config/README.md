# Vulkan Asciidoc Configuration Files

## Config File Macros

The macros in `vkspec.conf` and `manpages.conf` are described in the "Vulkan
Documentation and Extensions: Procedures and Conventions" document (see the
[styleguide](../styleguide.txt)). There's a parsing issue with comments in
`vkspec.conf`, so they are pulled out here for now.

## PDF Generation

`vkspec-dblatex.xsl` is XSL specific to a2x->dblatex->PDF spec generation.
It is a very slightly modified version of
/etc/asciidoc/dblatex/asciidoc-dblatex.xsl, as follows:

```
<xsl:param name="latex.hyperparam">colorlinks,linkcolor=black,pdfstartview=FitH</xsl:param>
<xsl:param name="doc.publisher.show">0</xsl:param>
<xsl:param name="latex.output.revhistory">0</xsl:param>
```

and has been simplified to just those parameters. Additional templates
replacing those under `/usr/share/xml/docbook/stylesheet/dblatex/xsl/*`
can be added here.

## HTML Generation

`vkspec-html.css` is CSS for HTML generated by the asciidoc xhtml11 and
html5 backends. It is branched from `/etc/asciidoc/stylesheets/asciidoc.css`
and tweaked to more closely resemble the asciidoc Docbook XHTML stylesheet.
This gives us direct control over CSS for the document, including support
for markup styles.

## Asciidoc Stylesheets

`docbook-xsl/common.xsl` replaces parts of the asciibook stylesheets
normally found under `/etc/asciidoc/docbook-xsl` in order to generate
consistent IDs on sections. We no longer support the docbook toolchain for
anything except PDF output, so other specializations have been removed.

## Support for Math

`math-asciidoc.conf`, `math-docbook.conf`, and `math.js` customize asciidoc
macros for HTML5 and Docbook output to insert KaTeX `<script>` tags from
`math.js` for HTML5, and properly pass through math which has
`\begin{}\/end{}` delimiters instead of $$\[\]\(\), using the
`<?texmath delimiters="user"?>` processing instruction.

`math-docbook.conf` is heavily conditionalized depending on whether the
final output format (which should be described in the a2x-format variable)
is `pdf` or not, since Docbook passes through math differently to dblatex
vs. the XHTML stylesheets. This could be simplified now that we're only
using Docbook for PDFs.