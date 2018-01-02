# doidata

[![Project Status: Concept – Minimal or no implementation has been done yet, or the repository is only intended to be a limited example, demo, or proof-of-concept.](http://www.repostatus.org/badges/latest/concept.svg)](http://www.repostatus.org/#concept)

Simple repository data access

_This is mostly an idea I am hoping others will pick up - Noam_

# Introduction

At rOpenSci and in associated open science groups, we often encourage scientists
to deposit and use data in public repositories that have stable, long-term archival
infrastructure and robust metadata.  Such repositories include Zenodo, Figshare,
Dryad, and a variety of more specific ones. A frequent mode of use is to download files from
these repositories, break the link with the original version or metadata, and
include some portion or derived form of these data in a new project folder.  This
leads to fragmentation of data.

One of the reasons for this use mode is that API navigation of these repositories
can be daunting or overly complex.  On the other hand, R data packages are a popular way to distribute data to make it very easy to use, but this is R-specific and breaks connections to archival repositories.

The aim of this package is to simplify
the workflow of using archived data to a single line, like so:

`my_data <- datadoi::get_data("10.1234/somerecord987/FILENAME.csv")`

This would parse the DOI, navigate the repository API (Figshare, Zenodo, Dryad, Open Science Framework, etc.) to find the associated file, and download it.  If the repository has metadata describing how the data should be parsed, it will be used.  Otherwise it can guess using [**rio**](https://github.com/leeper/rio) or take an argument to return the information raw or write it to disk.

## Some notes:

-  Non-archival/DOI-granting sources (GitHub, data.world) could be supported,
these would be secondary as the goal would be encourage use archival repositories
-  Versioning would be handled on the repository side, though `get_data()` could take a
   `version=` argument for those repos that have versions but not versioned DOIs (e.g., Figshare)
-  This differes from [**datastorr**](https://ropenscilabs.github.io/datastorr/) in that it's not a framework
   for versioning data, and it seeks to _avoid_ creating new packages for data.  It could borrow some of **datastorr** cacheing components, though.
-  Download cacheing would be optional
-  Github-linked Zenodo repositories unfortunately don't store files individually, but as a single
   ZIP of the GitHub release.  The package should detect this, download, (cache), unzip and retrieve
   individual files automatically.
-  There might an option or function to return a citation, too, though mostly the idea here is
that by keeping the DOI in the code you maintain a conceptual link the original record.
-  This probably requires up-to-date packages on the key repositories (Figshare, Zenodo, OSF. Dryad, DataONE), though quick-and-dirty methods might be doable for some repositories rather than wait for
full-blown API clients.
