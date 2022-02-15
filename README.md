## Background

In 2019, a joint project called the “Synthesis of Gully MPA
Oceanographic Monitoring Data” was initiated between Fisheries and
Oceans Canada (DFO)’s Maritimes Region Canadian Science Advisory
Secretariat (CSAS) and the Atlantic Zone Monitoring Program (AZMP). The
aim of this project was to compile the physical, chemical and biological
oceanographic data collected by the AZMP in the Gully MPA, and provide
preliminary analyses of patterns in these conditions over a near 20-year
period. Given that this represented the first synthesis and preliminary
analysis of AZMP data that are biannually collected within the Gully
MPA, one of the primary deliverables of this project was to create and
archive reproducible R code that could be used to A) produce a template
for reproducible analytical products designed to evaluate changes in
oceanographic conditions in the MPA, and B) create a reproducible
technical report to facilitate consistent reporting of these trends, as
new data are collected.

R package [csasdown](https://github.com/pbs-assess/csasdown) was
recently developed by members of DFO’s Pacific Region to facilitate the
creation of reproducible CSAS documents using **R Markdown** and
**bookdown**. The package also includes scripts for generating
reproducible DFO technical reports (see [Guide for the production of
Fisheries and Oceans Canada science report
series](https://publications.gc.ca/site/eng/9.874714/publication.html)
for more information).

The purpose of these instructions is two-fold: A) to provide a basic
demonstration to new users of **csasdown** on how to create a
reproducible DFO technical report, and B) to provide a guide to the
scripts used to create the Canadian Technical Report of Hydrography and
Ocean Sciences report titled [“Ocean monitoring of the Gully MPA - A
synopsis of data collected by the Atlantic Zone Monitoring Program”](https://cat.fsl-bsf.scitech.gc.ca/record=b4101498~S1).

It is our hope that future initiatives aimed at analyzing and reporting
on the oceanographic conditions of the Gully MPA could utilize these
instructions and code to produce a report with consistent analytical
products and formatting.

</br>

## Demonstration of how to Create a Template DFO Technical Report using csasdown

The **csasdown** [GitHub
website](https://github.com/pbs-assess/csasdown) provides a README.md
and PDF presentation on the use of the package to create DFO CSAS
reports. The various files and subfolders that are included with the
**csasdown** package are explained in the README.md. Below is a quick
demonstration on the creation of a blank/template DFO technical report
produced using **csasdown**.

### Setting the Work Environment

We recommend using [RStudio](https://rstudio.com) as the integrated
development environment (IDE). If the user is using Windows then it is
highly recommended to also install
[Rtools](https://cran.r-project.org/bin/windows/Rtools/), which are
tools to compile R packages from source; a feature that is occasionally
required.

The **renv** package is used to create a new R environment containing
only base R packages, and all of the additional R packages required to
create the technical report.

**Step 1.** Created a new R project (we called this
**ReproducibleReport**) in a new directory of the same name. This is
done by selecting the menu item **File -\> New Project** which open the
New Project wizard (Figure 1).

<img src="readme-figures/Figure-01-Select-New-Project-From-Menu.png" width="1440" style="display: block; margin: auto;" />

</br>

**Step 2.** Click the New Directory option.

<img src="readme-figures/Figure-02-Select-New-Directory.png" width="535" style="display: block; margin: auto;" />

</br>

**Step 3.** Click the New Project option.

<img src="readme-figures/Figure-03-Select-New-Project.png" width="535" style="display: block; margin: auto;" />

</br>

**Step 4.** Check the boxes next to the options “Create a git
repository” and “Use renv with this project” before pressing the
**Create Project** button. Note the “Open in a new session” option only
needs to be checked if the user was already in an active R project.

<img src="readme-figures/Figure-04-CreateNewProjectWithRenvAndGit.png" width="536" style="display: block; margin: auto;" />

These options were selected to:

-   start the project with a clean R environment containing only the R
    base packages (**renv**), and
-   allow the tracking of all changes to the code and files (**git**).

A new R environment will open. The following figures show how the
environment looks within RStudio and in the file browser.

<img src="readme-figures/Figure-05-EmptyProjectEnvironment.png" width="1536" style="display: block; margin: auto;" />

</br>

<img src="readme-figures/Figure-06-CleanProjectSetup.png" width="1184" style="display: block; margin: auto;" />

</br>

Re-creating a reproducible report requires the user to have knowledge of
what R packages were used and their version numbers. R package **renv**
tracks this information automatically with little user involvement.

One of the useful functions of **renv** is being able to see if your
environment has changed; i.e., it is out of sync with the lockfile (a
text file storing information about the packages that have been
installed in the environment). Below is the current contents of the
lockfile (*renv.lock*). Notice the only project package installed is
**renv**.

``` r
{
  "R": {
    "Version": "4.0.5",
    "Repositories": [
      {
        "Name": "CRAN",
        "URL": "https://cran.rstudio.com"
      }
    ]
  },
  "Packages": {
    "renv": {
      "Package": "renv",
      "Version": "0.13.2",
      "Source": "Repository",
      "Repository": "CRAN",
      "Hash": "079cb1f03ff972b30401ed05623cbe92"
    }
  }
}
```

To check if the environment is out of sync, execute the `renv::status`
command below. You will see that the project is synced with the
lockfile.

``` r
> renv::status()
* The project is already synchronized with the lockfile.
```

</br>

### Package Installation

**Step 5.** Now it is time to add the R packages required to create the
reproducible report. Run the following commands to install the
**remotes** and **csasdown** packages. The **remotes** package allows
you to install remote repositories (i.e., from GitHub).

``` r
> install.packages("remotes")
Installing remotes [2.2.0] ...
    OK [copied local binary]

> remotes::install_github("pbs-assess/csasdown")

Downloading GitHub repo pbs-assess/csasdown@HEAD
Downloading GitHub repo pbs-assess/rosettafish@HEAD
Installing 4 packages: stringi, magrittr, glue, stringr
Installing packages into ‘C:/DEV/GullyReproducibleReport/renv/library/R-4.0/x86_64-w64-mingw32’
(as ‘lib’ is unspecified)
trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.0/stringi_1.5.3.zip'
Content type 'application/zip' length 15243599 bytes (14.5 MB)
downloaded 14.5 MB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.0/magrittr_2.0.1.zip'
Content type 'application/zip' length 235751 bytes (230 KB)
downloaded 230 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.0/glue_1.4.2.zip'
Content type 'application/zip' length 154900 bytes (151 KB)
downloaded 151 KB

trying URL 'https://cran.rstudio.com/bin/windows/contrib/4.0/stringr_1.4.0.zip'
Content type 'application/zip' length 216733 bytes (211 KB)
downloaded 211 KB

package ‘stringi’ successfully unpacked and MD5 sums checked
package ‘magrittr’ successfully unpacked and MD5 sums checked
package ‘glue’ successfully unpacked and MD5 sums checked
package ‘stringr’ successfully unpacked and MD5 sums checked

… lots of additional information was output but the important info was that the following packages were installed:

package ‘digest’ successfully unpacked and MD5 sums checked
package ‘glue’ successfully unpacked and MD5 sums checked
package ‘assertthat’ successfully unpacked and MD5 sums checked
package ‘vctrs’ successfully unpacked and MD5 sums checked
package ‘rlang’ successfully unpacked and MD5 sums checked
package ‘lifecycle’ successfully unpacked and MD5 sums checked
package ‘fansi’ successfully unpacked and MD5 sums checked
package ‘ellipsis’ successfully unpacked and MD5 sums checked
package ‘crayon’ successfully unpacked and MD5 sums checked
package ‘cli’ successfully unpacked and MD5 sums checked
package ‘pkgconfig’ successfully unpacked and MD5 sums checked
package ‘mime’ successfully unpacked and MD5 sums checked
package ‘xfun’ successfully unpacked and MD5 sums checked
package ‘cpp11’ successfully unpacked and MD5 sums checked
package ‘systemfonts’ successfully unpacked and MD5 sums checked
package ‘ps’ successfully unpacked and MD5 sums checked
package ‘R6’ successfully unpacked and MD5 sums checked
package ‘processx’ successfully unpacked and MD5 sums checked
package ‘callr’ successfully unpacked and MD5 sums checked
package ‘jsonlite’ successfully unpacked and MD5 sums checked
package ‘magrittr’ successfully unpacked and MD5 sums checked
package ‘sys’ successfully unpacked and MD5 sums checked
package ‘stringi’ successfully unpacked and MD5 sums checked
package ‘askpass’ successfully unpacked and MD5 sums checked
package ‘pillar’ successfully unpacked and MD5 sums checked
package ‘stringr’ successfully unpacked and MD5 sums checked
package ‘openssl’ successfully unpacked and MD5 sums checked
package ‘curl’ successfully unpacked and MD5 sums checked
package ‘xml2’ successfully unpacked and MD5 sums checked
package ‘tibble’ successfully unpacked and MD5 sums checked
package ‘selectr’ successfully unpacked and MD5 sums checked
package ‘httr’ successfully unpacked and MD5 sums checked
package ‘colorspace’ successfully unpacked and MD5 sums checked
package ‘viridisLite’ successfully unpacked and MD5 sums checked
package ‘RColorBrewer’ successfully unpacked and MD5 sums checked
package ‘munsell’ successfully unpacked and MD5 sums checked
package ‘labeling’ successfully unpacked and MD5 sums checked
package ‘farver’ successfully unpacked and MD5 sums checked
package ‘purrr’ successfully unpacked and MD5 sums checked
package ‘markdown’ successfully unpacked and MD5 sums checked
package ‘highr’ successfully unpacked and MD5 sums checked
package ‘base64enc’ successfully unpacked and MD5 sums checked
package ‘tinytex’ successfully unpacked and MD5 sums checked
package ‘evaluate’ successfully unpacked and MD5 sums checked
package ‘htmltools’ successfully unpacked and MD5 sums checked
package ‘yaml’ successfully unpacked and MD5 sums checked
package ‘knitr’ successfully unpacked and MD5 sums checked
package ‘BH’ successfully unpacked and MD5 sums checked
package ‘hms’ successfully unpacked and MD5 sums checked
package ‘clipr’ successfully unpacked and MD5 sums checked
package ‘uuid’ successfully unpacked and MD5 sums checked
package ‘zip’ successfully unpacked and MD5 sums checked
package ‘svglite’ successfully unpacked and MD5 sums checked
package ‘webshot’ successfully unpacked and MD5 sums checked
package ‘rstudioapi’ successfully unpacked and MD5 sums checked
package ‘scales’ successfully unpacked and MD5 sums checked
package ‘rmarkdown’ successfully unpacked and MD5 sums checked
package ‘rvest’ successfully unpacked and MD5 sums checked
package ‘withr’ successfully unpacked and MD5 sums checked
package ‘isoband’ successfully unpacked and MD5 sums checked
package ‘gtable’ successfully unpacked and MD5 sums checked
package ‘tidyselect’ successfully unpacked and MD5 sums checked
package ‘generics’ successfully unpacked and MD5 sums checked
package ‘readr’ successfully unpacked and MD5 sums checked
package ‘officer’ successfully unpacked and MD5 sums checked
package ‘kableExtra’ successfully unpacked and MD5 sums checked
package ‘ggplot2’ successfully unpacked and MD5 sums checked
package ‘dplyr’ successfully unpacked and MD5 sums checked
package ‘bookdown’ successfully unpacked and MD5 sums checked
```

Note that if the package is already stored on your computer, a message
may appear that says **OK \[copied local binary\]** when installing the
package in a new R environment. If the package is not locally stored
then it will be downloaded from the default R package repository or the
repository specified in the function call if retrieving it from
[GitHub](https://github.com/), for example.

Now that a host of new packages were installed, check if the environment
is out of sync:

``` r
> renv::status()
* The project is already synchronized with the lockfile.
```

Surprisingly, the project is not out of sync with the lockfile after
installing new packages. This is because none of project’s current files
depend on the new packages to compile (run). Thus the environment is
officially unchanged; although new packages are stored within the
**renv** subfolder.

</br>

**Step 6.** Install the version of LaTeX that will be used to generate
the PDF version of the report. This wiki page provides instructions for
installing LaTeX for use with **csasdown**:
<https://github.com/pbs-assess/csasdown/wiki/Latex-Installation-for-csasdown>.

``` r
> tinytex::install_tinytex()
trying URL 'https://yihui.org/tinytex/TinyTeX-1.zip'
Content type 'application/octet-stream' length 103777374 bytes (99.0 MB)
downloaded 99.0 MB
```

The outputs from the installation of **tinytex** may vary, but should
resemble the following:

``` r
tlmgr install IEEEtran adjustbox algorithmicx algorithms appendix arydshln babel-english blindtext bookmark caption chngcntr cite collectbox colortbl datetime2 datetime2-english doi enumitem environ eso-pic etex-pkg everypage fancyhdr fp grfext hanging ifmtarg ifnextok import lastpage linegoal lineno lipsum luatex85 makecell marginnote microtype ms multirow ncctools nowidow oberdiek parskip pdfcomment pdfcrop pdflscape pdfpages pgf placeins ragged2e realboxes relsize rsfs scalerel sectsty setspace siunitx soul soulpos soulutf8 subfig tabu textcase textpos threeparttable threeparttablex titlecaps tlshell tocbibind tocloft tracklang trimspaces ulem varwidth was wrapfig xifthen xltabular zref
tlmgr.pl install: package IEEEtran not present in repository.
tlmgr.pl: package repository https://www.ctan.org/tex-archive/systems/texlive/tlnet (not verified: gpg unavailable)
[1/80, ??:??/??:??] install: adjustbox [14k]
[2/80, 00:03/15:57] install: algorithmicx [8k]
[3/80, 00:07/23:44] install: algorithms [4k]
[4/80, 00:10/29:10] install: appendix [3k]
[5/80, 00:13/33:58] install: arydshln [10k]
[6/80, 00:26/50:47] install: babel-english [3k]
[7/80, 00:30/54:09] install: blindtext [11k]
[8/80, 00:33/47:10] install: bookmark [9k]
[9/80, 00:46/56:31] install: caption [31k]
[10/80, 01:13/58:52] install: chngcntr [3k]
[11/80, 01:27/01:08:35] install: cite [20k]
[12/80, 01:40/01:04:39] install: collectbox [3k]
[13/80, 01:43/01:04:55] install: colortbl [4k]
TLPDB::_install_data: downloading did not succeed (download_file failed) for https://www.ctan.org/tex-archive/systems/texlive/tlnet/archive/colortbl.tar.xz
[14/80, 03:37/02:12:58] install: datetime2 [9k]
[15/80, 03:41/02:05:59] install: datetime2-english [6k]
[16/80, 03:44/02:02:01] install: doi [3k]
[17/80, 04:10/02:14:01] install: enumitem [14k]
[18/80, 04:13/02:03:04] install: environ [2k]
[19/80, 04:39/02:14:01] install: eso-pic [4k]
[20/80, 04:42/02:12:06] install: etex-pkg [6k]
[21/80, 04:46/02:08:55] install: everypage [2k]
[22/80, 04:50/02:09:18] install: fancyhdr [5k]
[23/80, 04:53/02:06:38] install: fp [19k]
[24/80, 04:56/01:54:44] install: grfext [3k]
[25/80, 04:58/01:54:04] install: hanging [2k]
[26/80, 05:01/01:54:02] install: ifmtarg [1k]
[27/80, 05:04/01:54:47] install: ifnextok [11k]
[28/80, 05:17/01:53:19] install: import [3k]
[29/80, 05:20/01:52:53] install: lastpage [4k]
[30/80, 05:23/01:51:59] install: linegoal [2k]
[31/80, 05:26/01:51:58] install: lineno [61k]
[32/80, 05:29/01:27:11] install: lipsum [74k]
[33/80, 05:42/01:10:58] install: luatex85 [2k]
[34/80, 05:45/01:11:14] install: makecell [5k]
[35/80, 05:48/01:10:57] install: marginnote [4k]
[36/80, 05:52/01:11:06] install: microtype [46k]
[37/80, 05:55/01:03:27] install: ms [3k]
[38/80, 05:58/01:03:31] install: multirow [3k]
[39/80, 06:01/01:03:36] install: ncctools [24k]
[40/80, 06:26/01:04:10] install: nowidow [2k]
[41/80, 06:30/01:04:39] install: oberdiek [45k]
[42/80, 06:33/59:02] install: parskip [3k]
[43/80, 06:36/59:09] install: pdfcomment [12k]
[44/80, 06:39/58:14] install: pdfcrop.win32 [1k]
[45/80, 06:51/59:54] install: pdfcrop [11k]
[46/80, 06:54/59:02] install: pdflscape [3k]
[47/80, 06:57/59:12] install: pdfpages [14k]
[48/80, 07:01/58:10] install: pgf [701k]
[49/80, 07:07/24:52] install: placeins [3k]
[50/80, 07:11/25:03] install: ragged2e [3k]
[51/80, 07:14/25:10] install: realboxes [3k]
[52/80, 07:28/25:55] install: relsize [6k]
[53/80, 07:31/25:58] install: rsfs [55k]
[54/80, 07:34/25:02] install: scalerel [3k]
[55/80, 07:37/25:08] install: sectsty [5k]
[56/80, 07:41/25:16] install: setspace [8k]
[57/80, 07:55/25:53] install: siunitx [61k]
[58/80, 07:58/24:53] install: soul [6k]
[59/80, 08:01/24:56] install: soulpos [4k]
[60/80, 08:04/25:02] install: soulutf8 [4k]
[61/80, 08:07/25:07] install: subfig [7k]
[62/80, 08:11/25:12] install: tabu [24k]
[63/80, 08:14/24:56] install: textcase [2k]
[64/80, 08:26/25:31] install: textpos [5k]
[65/80, 08:29/25:35] install: threeparttable [6k]
[66/80, 08:32/25:39] install: threeparttablex [3k]
[67/80, 08:36/25:48] install: titlecaps [5k]
[68/80, 08:39/25:53] install: tlshell.win32 [2723k]
[69/80, 08:54/09:06] install: tlshell [29k]
[70/80, 08:57/09:05] install: tocbibind [3k]
[71/80, 09:24/09:32] install: tocloft [7k]
[72/80, 09:27/09:35] install: tracklang [19k]
[73/80, 09:30/09:35] install: trimspaces [1k]
[74/80, 09:33/09:38] install: ulem [7k]
[75/80, 09:59/10:03] install: varwidth [5k]
[76/80, 10:02/10:06] install: was [4k]
[77/80, 10:05/10:08] install: wrapfig [10k]
[78/80, 10:08/10:10] install: xifthen [3k]
[79/80, 10:11/10:13] install: xltabular [2k]
[80/80, 10:14/10:16] install: zref [15k]
tlmgr.pl: action install returned an error; continuing.
running mktexlsr ...
done running mktexlsr.
running updmap-sys ...
tlmgr.pl: An error has occurred. See above messages. Exiting.
done running updmap-sys.
tlmgr.pl: package log updated: C:/Users/JacksonJ/AppData/Roaming/TinyTeX/texmf-var/web2c/tlmgr.log
tlmgr.pl: command log updated: C:/Users/JacksonJ/AppData/Roaming/TinyTeX/texmf-var/web2c/tlmgr-commands.log
tlmgr update --self
tlmgr install IEEEtran adjustbox algorithmicx algorithms appendix arydshln babel-english blindtext bookmark caption chngcntr cite collectbox colortbl datetime2 datetime2-english doi enumitem environ eso-pic etex-pkg everypage fancyhdr fp grfext hanging ifmtarg ifnextok import lastpage linegoal lineno lipsum luatex85 makecell marginnote microtype ms multirow ncctools nowidow oberdiek parskip pdfcomment pdfcrop pdflscape pdfpages pgf placeins ragged2e realboxes relsize rsfs scalerel sectsty setspace siunitx soul soulpos soulutf8 subfig tabu textcase textpos threeparttable threeparttablex titlecaps tlshell tocbibind tocloft tracklang trimspaces ulem varwidth was wrapfig xifthen xltabular zref
tlmgr.pl install: package IEEEtran not present in repository.
tlmgr.pl: package repository https://www.ctan.org/tex-archive/systems/texlive/tlnet (not verified: gpg unavailable)
tlmgr.pl install: package already present: adjustbox
tlmgr.pl install: package already present: algorithmicx
tlmgr.pl install: package already present: algorithms
tlmgr.pl install: package already present: appendix
tlmgr.pl install: package already present: arydshln
tlmgr.pl install: package already present: babel-english
tlmgr.pl install: package already present: blindtext
tlmgr.pl install: package already present: bookmark
tlmgr.pl install: package already present: caption
tlmgr.pl install: package already present: chngcntr
tlmgr.pl install: package already present: cite
tlmgr.pl install: package already present: collectbox
tlmgr.pl install: package already present: datetime2
tlmgr.pl install: package already present: datetime2-english
tlmgr.pl install: package already present: doi
tlmgr.pl install: package already present: enumitem
tlmgr.pl install: package already present: environ
tlmgr.pl install: package already present: eso-pic
tlmgr.pl install: package already present: etex-pkg
tlmgr.pl install: package already present: everypage
tlmgr.pl install: package already present: fancyhdr
tlmgr.pl install: package already present: fp
tlmgr.pl install: package already present: grfext
tlmgr.pl install: package already present: hanging
tlmgr.pl install: package already present: ifmtarg
tlmgr.pl install: package already present: ifnextok
tlmgr.pl install: package already present: import
tlmgr.pl install: package already present: lastpage
tlmgr.pl install: package already present: linegoal
tlmgr.pl install: package already present: lineno
tlmgr.pl install: package already present: lipsum
tlmgr.pl install: package already present: luatex85
tlmgr.pl install: package already present: makecell
tlmgr.pl install: package already present: marginnote
tlmgr.pl install: package already present: microtype
tlmgr.pl install: package already present: ms
tlmgr.pl install: package already present: multirow
tlmgr.pl install: package already present: ncctools
tlmgr.pl install: package already present: nowidow
tlmgr.pl install: package already present: oberdiek
tlmgr.pl install: package already present: parskip
tlmgr.pl install: package already present: pdfcomment
tlmgr.pl install: package already present: pdfcrop
tlmgr.pl install: package already present: pdflscape
tlmgr.pl install: package already present: pdfpages
tlmgr.pl install: package already present: pgf
tlmgr.pl install: package already present: placeins
tlmgr.pl install: package already present: ragged2e
tlmgr.pl install: package already present: realboxes
tlmgr.pl install: package already present: relsize
tlmgr.pl install: package already present: rsfs
tlmgr.pl install: package already present: scalerel
tlmgr.pl install: package already present: sectsty
tlmgr.pl install: package already present: setspace
tlmgr.pl install: package already present: siunitx
tlmgr.pl install: package already present: soul
tlmgr.pl install: package already present: soulpos
tlmgr.pl install: package already present: soulutf8
tlmgr.pl install: package already present: subfig
tlmgr.pl install: package already present: tabu
tlmgr.pl install: package already present: textcase
tlmgr.pl install: package already present: textpos
tlmgr.pl install: package already present: threeparttable
tlmgr.pl install: package already present: threeparttablex
tlmgr.pl install: package already present: titlecaps
tlmgr.pl install: package already present: tlshell
tlmgr.pl install: package already present: tocbibind
tlmgr.pl install: package already present: tocloft
tlmgr.pl install: package already present: tracklang
tlmgr.pl install: package already present: trimspaces
tlmgr.pl install: package already present: ulem
tlmgr.pl install: package already present: varwidth
tlmgr.pl install: package already present: was
tlmgr.pl install: package already present: wrapfig
tlmgr.pl install: package already present: xifthen
tlmgr.pl install: package already present: xltabular
tlmgr.pl install: package already present: zref
[1/1, ??:??/??:??] install: colortbl [4k]
tlmgr.pl: action install returned an error; continuing.
running mktexlsr ...
done running mktexlsr.
tlmgr.pl: An error has occurred. See above messages. Exiting.
tlmgr.pl: package log updated: C:/Users/JacksonJ/AppData/Roaming/TinyTeX/texmf-var/web2c/tlmgr.log
tlmgr.pl: command log updated: C:/Users/JacksonJ/AppData/Roaming/TinyTeX/texmf-var/web2c/tlmgr-commands.log
```

</br>

### Creating a Reproducible DFO Technical Report

**Step 7.** The `draft` function of R package **csasdown** is a wrapper
around the `rmarkdown::draft` function, and allows the user to create
reports that adhere to the template outlined by DFO CSAS publications
(csasdown::draft(“resdoc”) and (csasdown::draft(“sr”) for Research
Documents and Science Responses, respectively) and DFO secondary science
publications (csasdown::draft(“techreport”).

Secondary science publications include the DFO series ‘Canadian
Technical Report of Fisheries and Aquatic Sciences’, ‘Canadian Data
Report of Fisheries and Aquatic Sciences’, ‘Canadian Industry Report of
Fisheries and Aquatic Sciences’, ‘Canadian Manuscript Report of
Fisheries and Aquatic Sciences’, and ‘Canadian Hydrography and Ocean
Sciences – Technical Reports’, among other series. These technical
reports often contain preliminary scientific findings, and the
Department normally provides the only review for these documents. For
more information on DFO’s secondary publication series, please refer to:
<http://intra.dfo-mpo.gc.ca/science/publications/policy-politique/index-eng.html#24>

Execute the `draft` function, setting “techreport” as the ‘type’:

``` r
> csasdown::draft("techreport")
```

You will now see that a number of R markdown files and subfolders were
unpacked into both the R environment and project directory.

<img src="readme-figures/Figure-07-csasfiles.png" width="276" style="display: block; margin: auto;" />

Among these files includes several .Rmd files: *01_introduction.Rmd*,
*02_methods.Rmd*, *03_results.Rmd*, *04_discussion.Rmd*,
*05_appendix.Rmd*, and *06_bibliography.Rmd*. These .Rmd files form the
various chapters of the technical report (introduction, methods,
results, conclusion, etc.). If starting a new report, the user would
simply write the text of each chapter directly into the .Rmd files,
instead of using a word processor.

The index.Rmd file then ‘knits’ all the individual .Rmd chapters
together to create a single Markdown (.md) file. This Markdown file is
then converted to a LaTeX (.tex) file, and this .tex file is translated
into the final PDF technical report document.

The ***\_bookdown.yml*** file specifies which .Rmd files will be ‘knit’
when you press the knit button in ***index.Rmd***:

``` r
book_filename: "techreport"
rmd_files: ["index.Rmd",
            "01_introduction.Rmd",
            "02_methods.Rmd",
            "03_results.Rmd",
            "04_discussion.Rmd",
            "05_appendix.Rmd",
            "06_bibliography.Rmd"]
delete_merged_file: true
```

When creating a new technical report, the user can rename these files to
a custom naming convention, or add additional chapters as required. The
\*\*\_bookdown.yml\*\* file is very useful because you can comment out
one or more lines/chapters so when the project is knit, only certain
chapters are included. This allows the user to slowly build the report,
checking the resulting output as content is added sequentially.

</br>

Once `draft("techreport")` is executed, the R environment becomes out of
sync. Use `renv::status()` to check the status of the environment:

``` r
> renv::status()
The following package(s) are installed but not recorded in the lockfile:
               _
  svglite        [2.0.0]
  lattice        [0.20-41]
  ps             [1.6.0]
  digest         [0.6.27]
  utf8           [1.2.1]
  mime           [0.11]
  R6             [2.5.0]
  csasdown       [pbs-assess/csasdown@HEAD]
  sys            [3.4]
  evaluate       [0.14]
  httr           [1.4.2]
  ggplot2        [3.3.5]
  highr          [0.9]
  pillar         [1.6.1]
  rlang          [0.4.11]
  uuid           [0.1-4]
  curl           [4.3.2]
  rstudioapi     [0.13]
  callr          [3.7.0]
  Matrix         [1.3-2]
  rmarkdown      [2.9]
  labeling       [0.4.2]
  webshot        [0.5.2]
  readr          [1.4.0]
  stringr        [1.4.0]
  selectr        [0.4-2]
  munsell        [0.5.0]
  tinytex        [0.32]
  xfun           [0.24]
  pkgconfig      [2.0.3]
  askpass        [1.1]
  systemfonts    [1.0.2]
  base64enc      [0.1-3]
  clipr          [0.7.1]
  mgcv           [1.8-34]
  htmltools      [0.5.1.1]
  openssl        [1.4.4]
  tidyselect     [1.1.1]
  tibble         [3.1.2]
  bookdown       [0.22]
  fansi          [0.5.0]
  viridisLite    [0.4.0]
  crayon         [1.4.1]
  dplyr          [1.0.7]
  withr          [2.4.2]
  MASS           [7.3-53.1]
  BH             [1.75.0-0]
  nlme           [3.1-152]
  jsonlite       [1.7.2]
  gtable         [0.3.0]
  lifecycle      [1.0.0]
  magrittr       [2.0.1]
  scales         [1.1.1]
  zip            [2.2.0]
  cli            [3.0.0]
  stringi        [1.6.2]
  farver         [2.1.0]
  xml2           [1.3.2]
  ellipsis       [0.3.2]
  generics       [0.1.0]
  vctrs          [0.3.8]
  kableExtra     [1.3.4]
  RColorBrewer   [1.1-2]
  glue           [1.4.2]
  officer        [0.3.18]
  markdown       [1.1]
  purrr          [0.3.4]
  hms            [1.1.0]
  processx       [3.5.2]
  yaml           [2.2.1]
  colorspace     [2.0-2]
  rosettafish    [pbs-assess/rosettafish@HEAD]
  rvest          [1.0.0]
  isoband        [0.2.5]
  cpp11          [0.3.1]
  knitr          [1.33]
```

Use `renv::snapshot()` to add these packages to your lockfile, and to
save the current state of the project.

``` r
> renv::snapshot()
The following package(s) will be updated in the lockfile:

# CRAN ===============================
- BH             [* -> 1.75.0-0]
- MASS           [* -> 7.3-53.1]
- Matrix         [* -> 1.3-2]
- R6             [* -> 2.5.0]
- RColorBrewer   [* -> 1.1-2]
- askpass        [* -> 1.1]
- base64enc      [* -> 0.1-3]
- bookdown       [* -> 0.22]
- callr          [* -> 3.7.0]
- cli            [* -> 3.0.0]
- clipr          [* -> 0.7.1]
- colorspace     [* -> 2.0-2]
- cpp11          [* -> 0.3.1]
- crayon         [* -> 1.4.1]
- curl           [* -> 4.3.2]
- digest         [* -> 0.6.27]
- dplyr          [* -> 1.0.7]
- ellipsis       [* -> 0.3.2]
- evaluate       [* -> 0.14]
- fansi          [* -> 0.5.0]
- farver         [* -> 2.1.0]
- generics       [* -> 0.1.0]
- ggplot2        [* -> 3.3.5]
- glue           [* -> 1.4.2]
- gtable         [* -> 0.3.0]
- highr          [* -> 0.9]
- hms            [* -> 1.1.0]
- htmltools      [* -> 0.5.1.1]
- httr           [* -> 1.4.2]
- isoband        [* -> 0.2.5]
- jsonlite       [* -> 1.7.2]
- kableExtra     [* -> 1.3.4]
- knitr          [* -> 1.33]
- labeling       [* -> 0.4.2]
- lattice        [* -> 0.20-41]
- lifecycle      [* -> 1.0.0]
- magrittr       [* -> 2.0.1]
- markdown       [* -> 1.1]
- mgcv           [* -> 1.8-34]
- mime           [* -> 0.11]
- munsell        [* -> 0.5.0]
- nlme           [* -> 3.1-152]
- officer        [* -> 0.3.18]
- openssl        [* -> 1.4.4]
- pillar         [* -> 1.6.1]
- pkgconfig      [* -> 2.0.3]
- processx       [* -> 3.5.2]
- ps             [* -> 1.6.0]
- purrr          [* -> 0.3.4]
- readr          [* -> 1.4.0]
- rlang          [* -> 0.4.11]
- rmarkdown      [* -> 2.9]
- rstudioapi     [* -> 0.13]
- rvest          [* -> 1.0.0]
- scales         [* -> 1.1.1]
- selectr        [* -> 0.4-2]
- stringi        [* -> 1.6.2]
- stringr        [* -> 1.4.0]
- svglite        [* -> 2.0.0]
- sys            [* -> 3.4]
- systemfonts    [* -> 1.0.2]
- tibble         [* -> 3.1.2]
- tidyselect     [* -> 1.1.1]
- tinytex        [* -> 0.32]
- utf8           [* -> 1.2.1]
- uuid           [* -> 0.1-4]
- vctrs          [* -> 0.3.8]
- viridisLite    [* -> 0.4.0]
- webshot        [* -> 0.5.2]
- withr          [* -> 2.4.2]
- xfun           [* -> 0.24]
- xml2           [* -> 1.3.2]
- yaml           [* -> 2.2.1]
- zip            [* -> 2.2.0]

# GitHub =============================
- csasdown       [* -> pbs-assess/csasdown@HEAD]
- rosettafish    [* -> pbs-assess/rosettafish@HEAD]

Do you want to proceed? [y/N]: y
* Lockfile written to 'C:/DEV/ReproducibleReport/renv.lock'.
```

</br>

**Step 8.** Open the **index.Rmd** file in the project directory and
press the ‘Knit’ button. This will knit all the .Rmd report sections
together, to produce the final PDF report.

If the code has run successfully, a viewer should open that contains the
template technical report PDF. The .tex and .pdf report are also saved
in the \_book subfolder in the project directory.

The cover of the technical report (tech-report-cover.docx) must be
manually edited and the report type (e.g., Canadian Technical Report of
Fisheries and Aquatic Sciences, Canadian Technical Report of
Hydrographic and Ocean Sciences) changed, depending on which report
series is being targeted for publication.

If the code does not run successfully (probably indicated by missing
libraries with the file extension .sty) then it may be necessary to do a
full install of the full suite of TinyTex libraries using the command:

`tinytex::tlmgr_install('scheme-full')`

The installation of the full suite of TinyTex libraries will take quite
a while; possibly an hour or more.

And this concludes a demonstration of how to generate a reproducible DFO
technical report using R package **csasdown**!

</br>

## Replication of the Gully Oceanographic Monitoring DFO Technical Report

The repository of code, data, and figures used to create the Canadian
Technical Report of Hydrography and Ocean Sciences report titled
‘Oceanographic monitoring of the Gully MPA - A synopsis of data
collected by the Atlantic Zone Monitoring Program’ can be found here:
<https://github.com/AtlanticR/reproducible-gully-report>.

To reproduce the report stored in this repository, the user must have
[R](https://cran.r-project.org/), [RStudio](https://www.rstudio.com/)
and [Rtools](https://cran.r-project.org/bin/windows/Rtools/) installed
on their computer.

It also helps to eliminate possible errors by installing the devtools R
package.

**Step 1.** Download the repository from GitHub by clicking the Code
button, and ‘Download ZIP’ option. Unzip the contents of the project
directory.

You will see that the ‘chapter’ .Rmd files are slightly different from
those unpacked from `draft("techreport")`. Additional chapters were
added, and their naming convention customized to reflect the contents
presented in the Gully technical report. Users who wish to utilize this
code for future iterations of this report, or other reports designed to
evaluate changes in oceanographic conditions can re-write the text in
each .Rmd file.

<img src="readme-figures/Figure-08-GullyDirectory.png" width="372" style="display: block; margin: auto;" />

**Step 2.** Double-click on the R project titled
**GullyReproducibleReport.RProj**, or open the project from within
RStudio. This will open the R environment in which the project was
saved.

The first time the project is opened there will be a message (see
following) about the environment being out of sync with the lockfile
(see renv).

``` r
R version 4.0.5 (2021-03-31) -- "Shake and Throw"
Copyright (C) 2021 The R Foundation for Statistical Computing
Platform: x86_64-w64-mingw32/x64 (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

# Bootstrapping renv 0.13.2 --------------------------------------------------
* Downloading renv 0.13.2 ... OK (downloaded binary)
* Installing renv 0.13.2 ... Done!
* Successfully installed and loaded renv 0.13.2.
* Project 'C:/DEV/new-gully-report' loaded. [renv 0.13.2]
* The project library is out of sync with the lockfile.
* Use `renv::restore()` to install packages recorded in the lockfile.
```

To re-sync the environment, execute the following:

``` r
> renv::restore()
The following package(s) will be updated:

# CRAN ===============================
- BH             [* -> 1.75.0-0]
- DBI            [* -> 1.1.1]
- R6             [* -> 2.5.0]
- RColorBrewer   [* -> 1.1-2]
- Rcpp           [* -> 1.0.6]
- askpass        [* -> 1.1]
- base64enc      [* -> 0.1-3]
- bookdown       [* -> 0.22]
- brio           [* -> 1.1.1]
- callr          [* -> 3.6.0]
- classInt       [* -> 0.4-3]
- cli            [* -> 2.4.0]
- clipr          [* -> 0.7.1]
- colorspace     [* -> 2.0-0]
- cpp11          [* -> 0.2.7]
- crayon         [* -> 1.4.1]
- curl           [* -> 4.3]
- desc           [* -> 1.3.0]
- diffobj        [* -> 0.3.4]
- digest         [* -> 0.6.27]
- dplyr          [* -> 1.0.5]
- e1071          [* -> 1.7-6]
- ellipsis       [* -> 0.3.1]
- evaluate       [* -> 0.14]
- fansi          [* -> 0.4.2]
- farver         [* -> 2.1.0]
- generics       [* -> 0.1.0]
- ggplot2        [* -> 3.3.3]
- glue           [* -> 1.4.2]
- gsw            [* -> 1.0-5]
- gtable         [* -> 0.3.0]
- highr          [* -> 0.8]
- hms            [* -> 1.0.0]
- htmltools      [* -> 0.5.1.1]
- httr           [* -> 1.4.2]
- isoband        [* -> 0.2.4]
- jsonlite       [* -> 1.7.2]
- kableExtra     [* -> 1.3.4]
- knitr          [* -> 1.33]
- labeling       [* -> 0.4.2]
- lifecycle      [* -> 1.0.0]
- lubridate      [* -> 1.7.10]
- magrittr       [* -> 2.0.1]
- markdown       [* -> 1.1]
- mime           [* -> 0.10]
- munsell        [* -> 0.5.0]
- officer        [* -> 0.3.18]
- openssl        [* -> 1.4.3]
- pillar         [* -> 1.6.0]
- pkgconfig      [* -> 2.0.3]
- pkgload        [* -> 1.2.1]
- praise         [* -> 1.0.0]
- processx       [* -> 3.5.1]
- proxy          [* -> 0.4-25]
- ps             [* -> 1.6.0]
- purrr          [* -> 0.3.4]
- readr          [* -> 1.4.0]
- rematch2       [* -> 2.1.2]
- rlang          [* -> 0.4.10]
- rmarkdown      [* -> 2.8]
- rprojroot      [* -> 2.0.2]
- rstudioapi     [* -> 0.13]
- rvest          [* -> 1.0.0]
- scales         [* -> 1.1.1]
- selectr        [* -> 0.4-2]
- sf             [* -> 0.9-8]
- stringi        [* -> 1.5.3]
- stringr        [* -> 1.4.0]
- svglite        [* -> 2.0.0]
- sys            [* -> 3.4]
- systemfonts    [* -> 1.0.1]
- testthat       [* -> 3.0.2]
- tibble         [* -> 3.1.1]
- tidyselect     [* -> 1.1.0]
- tinytex        [* -> 0.31]
- units          [* -> 0.7-1]
- utf8           [* -> 1.2.1]
- uuid           [* -> 0.1-4]
- vctrs          [* -> 0.3.7]
- viridisLite    [* -> 0.3.0]
- waldo          [* -> 0.2.5]
- webshot        [* -> 0.5.2]
- withr          [* -> 2.4.1]
- xfun           [* -> 0.22]
- xml2           [* -> 1.3.2]
- yaml           [* -> 2.2.1]
- zip            [* -> 2.1.1]

# GitHub =============================
- csasdown       [* -> pbs-assess/csasdown@HEAD]
- oce            [* -> dankelley/oce@develop]
- rosettafish    [* -> pbs-assess/rosettafish@HEAD]

Do you want to proceed? [y/N]: y
Installing BH [1.75.0-0] ...
    OK [linked cache]
Installing DBI [1.1.1] ...
    OK [linked cache]
Installing R6 [2.5.0] ...
    OK [linked cache]
Installing RColorBrewer [1.1-2] ...
    OK [linked cache]
Installing Rcpp [1.0.6] ...
    OK [linked cache]
Installing sys [3.4] ...
    OK [linked cache]
Installing askpass [1.1] ...
    OK [linked cache]
Installing base64enc [0.1-3] ...
    OK [linked cache]
Installing digest [0.6.27] ...
    OK [linked cache]
Installing rlang [0.4.10] ...
    OK [linked cache]
Installing htmltools [0.5.1.1] ...
    OK [linked cache]
Installing evaluate [0.14] ...
    OK [linked cache]
Installing highr [0.8] ...
    OK [linked cache]
Installing xfun [0.22] ...
    OK [linked cache]
Installing mime [0.10] ...
    OK [linked cache]
Installing markdown [1.1] ...
    OK [linked cache]
Installing glue [1.4.2] ...
    OK [linked cache]
Installing magrittr [2.0.1] ...
    OK [linked cache]
Installing stringi [1.5.3] ...
    OK [linked cache]
Installing stringr [1.4.0] ...
    OK [linked cache]
Installing yaml [2.2.1] ...
    OK [linked cache]
Installing knitr [1.33] ...
    OK [linked cache]
Installing jsonlite [1.7.2] ...
    OK [linked cache]
Installing tinytex [0.31] ...
    OK [linked cache]
Installing rmarkdown [2.8] ...
    OK [linked cache]
Installing bookdown [0.22] ...
    OK [linked cache]
Installing brio [1.1.1] ...
    OK [linked cache]
Installing ps [1.6.0] ...
    OK [linked cache]
Installing processx [3.5.1] ...
    OK [linked cache]
Installing callr [3.6.0] ...
    OK [linked cache]
Installing proxy [0.4-25] ...
    OK [linked cache]
Installing e1071 [1.7-6] ...
    OK [linked cache]
Installing classInt [0.4-3] ...
    OK [linked cache]
Installing cli [2.4.0] ...
    OK [linked cache]
Installing clipr [0.7.1] ...
    OK [linked cache]
Installing colorspace [2.0-0] ...
    OK [linked cache]
Installing cpp11 [0.2.7] ...
    OK [linked cache]
Installing crayon [1.4.1] ...
    OK [linked cache]
Installing ellipsis [0.3.1] ...
    OK [linked cache]
Installing generics [0.1.0] ...
    OK [linked cache]
Installing lifecycle [1.0.0] ...
    OK [linked cache]
Installing fansi [0.4.2] ...
    OK [linked cache]
Installing utf8 [1.2.1] ...
    OK [linked cache]
Installing vctrs [0.3.7] ...
    OK [linked cache]
Installing pillar [1.6.0] ...
    OK [linked cache]
Installing pkgconfig [2.0.3] ...
    OK [linked cache]
Installing tibble [3.1.1] ...
    OK [linked cache]
Installing purrr [0.3.4] ...
    OK [linked cache]
Installing tidyselect [1.1.0] ...
    OK [linked cache]
Installing dplyr [1.0.5] ...
    OK [linked cache]
Installing gtable [0.3.0] ...
    OK [linked cache]
Installing isoband [0.2.4] ...
    OK [linked cache]
Installing farver [2.1.0] ...
    OK [linked cache]
Installing labeling [0.4.2] ...
    OK [linked cache]
Installing munsell [0.5.0] ...
    OK [linked cache]
Installing viridisLite [0.3.0] ...
    OK [linked cache]
Installing scales [1.1.1] ...
    OK [linked cache]
Installing withr [2.4.1] ...
    OK [linked cache]
Installing ggplot2 [3.3.3] ...
    OK [linked cache]
Installing xml2 [1.3.2] ...
    OK [linked cache]
Installing curl [4.3] ...
    OK [linked cache]
Installing openssl [1.4.3] ...
    OK [linked cache]
Installing httr [1.4.2] ...
    OK [linked cache]
Installing selectr [0.4-2] ...
    OK [linked cache]
Installing rvest [1.0.0] ...
    OK [linked cache]
Installing rstudioapi [0.13] ...
    OK [linked cache]
Installing webshot [0.5.2] ...
    OK [linked cache]
Installing systemfonts [1.0.1] ...
    OK [linked cache]
Installing svglite [2.0.0] ...
    OK [linked cache]
Installing kableExtra [1.3.4] ...
    OK [linked cache]
Installing zip [2.1.1] ...
    OK [linked cache]
Installing uuid [0.1-4] ...
    OK [linked cache]
Installing officer [0.3.18] ...
    OK [linked cache]
Installing hms [1.0.0] ...
    OK [linked cache]
Installing readr [1.4.0] ...
    OK [linked cache]
Installing rosettafish [0.0.0.9000] ...
    OK [linked cache]
Installing csasdown [0.0.10.9000] ...
    OK [linked cache]
Installing rprojroot [2.0.2] ...
    OK [linked cache]
Installing desc [1.3.0] ...
    OK [linked cache]
Installing diffobj [0.3.4] ...
    OK [linked cache]
Installing pkgload [1.2.1] ...
    OK [linked cache]
Installing praise [1.0.0] ...
    OK [linked cache]
Installing rematch2 [2.1.2] ...
    OK [linked cache]
Installing waldo [0.2.5] ...
    OK [linked cache]
Installing testthat [3.0.2] ...
    OK [linked cache]
Installing gsw [1.0-5] ...
    OK [linked cache]
Installing lubridate [1.7.10] ...
    OK [linked cache]
Installing units [0.7-1] ...
    OK [linked cache]
Installing sf [0.9-8] ...
    OK [linked cache]
Installing oce [1.4-0] ...
    OK [linked cache]
```

Next check the status of the environment (lock file) to see if
everything is now in sync.

``` r
> renv::status()
The following package(s) are out of sync:

    Package   Lockfile Version   Library Version
 KernSmooth            2.23-20           2.23-18
       MASS             7.3-54            7.3-53
     Matrix              1.3-4             1.3-2
       boot             1.3-28            1.3-26
      class             7.3-19            7.3-18
    cluster              2.1.2             2.1.0
    lattice            0.20-44           0.20-41
       mgcv             1.8-36            1.8-33
       nnet             7.3-16            7.3-15
        oce              1.5-0             1.4-0
    ocedata              0.1.9             0.1.8
    pkgload              1.2.1             1.2.2
    spatial             7.3-14            7.3-13
   survival             3.2-13             3.2-7

Use `renv::snapshot()` to save the state of your library to the lockfile.
Use `renv::restore()` to restore your library from the lockfile.
```

If it is not in sync then execute the `renv::snapshot` command to
synchronize the environment again.

``` r
> renv::snapshot()
```

At this step you may receive an error that R package **oce** failed to
install. This will be re-installed below once the **Index.Rmd** file is
opened.

</br>

**Step 4.** Install the **remotes** and **csasdown** packages:

``` r
> install.packages("remotes")
> remotes::install_github("pbs-assess/csasdown")
```

Using `renv::status`, you will see that the project is out of sync with
the lockfile. Use `renv::snapshot` to sync the project and lockfile.

</br>

**Step 5.** Open the index.Rmd file. An **R Markdown** pane will appear
in the R environment with the **Console**, **Terminal** and **Jobs**
panes visible. You may receive a message indicating that several
dependencies of the **Index.Rmd** file are not installed, including R
package **oce**. Select the **Install** option to install these
packages.

Use `renv::status()` to check the that the project is in sync with the
lockfile, and if not, execute `renv::snapshot()`.

Press the **Knit** button to compile the various .Rmd chapters into a
single report. The output in the R Markdown pane will resemble the
following:

``` r
processing file: techreport.Rmd
  |.                                                                     |   1%
  ordinary text without R code

  |..                                                                    |   2%
label: setup (with options) 
List of 5
 $ echo   : logi FALSE
 $ cache  : logi FALSE
 $ message: logi FALSE
 $ results: chr "hide"
 $ warning: logi FALSE

  |..                                                                    |   3%
  ordinary text without R code

  |...                                                                   |   4%
label: load-libraries (with options) 
List of 1
 $ cache: logi FALSE


Attaching package: 'dplyr'

The following objects are masked from 'package:stats':

    filter, lag

The following objects are masked from 'package:base':

    intersect, setdiff, setequal, union


Attaching package: 'kableExtra'

The following object is masked from 'package:dplyr':

    group_rows


Attaching package: 'lubridate'

The following objects are masked from 'package:base':

    date, intersect, setdiff, union

Loading required package: gsw
Loading required package: testthat

Attaching package: 'testthat'

The following object is masked from 'package:dplyr':

    matches

Loading required package: sf
Linking to GEOS 3.9.0, GDAL 3.2.1, PROJ 7.2.1

Attaching package: 'scales'

The following object is masked from 'package:readr':

    col_factor

The following object is masked from 'package:oce':

    rescale

  |....                                                                  |   6%
   inline R code fragments

  |.....                                                                 |   7%
label: table1 (with options) 
List of 3
 $ results: chr "asis"
 $ include: logi TRUE
 $ echo   : logi FALSE


Attaching package: 'tidyr'

The following object is masked from 'package:testthat':

    matches

  |......                                                                |   8%
  ordinary text without R code

  |......                                                                |   9%
label: table2 (with options) 
List of 3
 $ results: chr "asis"
 $ include: logi TRUE
 $ echo   : logi FALSE

  |.......                                                               |  10%
  ordinary text without R code

  |........                                                              |  11%
label: table3 (with options) 
List of 3
 $ results: chr "asis"
 $ include: logi TRUE
 $ echo   : logi FALSE

  |.........                                                             |  12%
  ordinary text without R code

  |.........                                                             |  13%
label: table4 (with options) 
List of 3
 $ results: chr "asis"
 $ include: logi TRUE
 $ echo   : logi FALSE

Joining, by = "years"
  |..........                                                            |  15%
  ordinary text without R code

  |...........                                                           |  16%
label: table5 (with options) 
List of 3
 $ results: chr "asis"
 $ include: logi TRUE
 $ echo   : logi FALSE

  |............                                                          |  17%
  ordinary text without R code

  |.............                                                         |  18%
label: table6 (with options) 
List of 3
 $ results: chr "asis"
 $ include: logi TRUE
 $ echo   : logi FALSE

  |.............                                                         |  19%
  ordinary text without R code

  |..............                                                        |  20%
label: table7 (with options) 
List of 3
 $ results: chr "asis"
 $ include: logi TRUE
 $ echo   : logi FALSE

  |...............                                                       |  21%
  ordinary text without R code

  |................                                                      |  22%
label: table8 (with options) 
List of 3
 $ results: chr "asis"
 $ include: logi TRUE
 $ echo   : logi FALSE

  |.................                                                     |  24%
  ordinary text without R code

  |.................                                                     |  25%
label: figure1 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig1-caption)"

  |..................                                                    |  26%
  ordinary text without R code

  |...................                                                   |  27%
label: figure2 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig2-caption)"

  |....................                                                  |  28%
  ordinary text without R code

  |....................                                                  |  29%
label: figure3 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig3-caption)"

  |.....................                                                 |  30%
  ordinary text without R code

  |......................                                                |  31%
label: figure4 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig4-caption)"

  |.......................                                               |  33%
  ordinary text without R code

  |........................                                              |  34%
label: figure5 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig5-caption)"
 $ out.width: chr "7.75in"

  |........................                                              |  35%
  ordinary text without R code

  |.........................                                             |  36%
label: figure6 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig6-caption)"
 $ out.width: chr "7.75in"

  |..........................                                            |  37%
  ordinary text without R code

  |...........................                                           |  38%
label: figure7 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig7-caption)"
 $ out.width: chr "7.75in"

  |............................                                          |  39%
  ordinary text without R code

  |............................                                          |  40%
label: figure8 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig8-caption)"
 $ out.width: chr "9.25in"

  |.............................                                         |  42%
  ordinary text without R code

  |..............................                                        |  43%
label: figure9 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig9-caption)"
 $ out.width: chr "9.0in"

  |...............................                                       |  44%
  ordinary text without R code

  |...............................                                       |  45%
label: figure10 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig10-caption)"

  |................................                                      |  46%
  ordinary text without R code

  |.................................                                     |  47%
label: figure11 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig11-caption)"

  |..................................                                    |  48%
  ordinary text without R code

  |...................................                                   |  49%
label: figure12 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig12-caption)"

  |...................................                                   |  51%
  ordinary text without R code

  |....................................                                  |  52%
label: figure13 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig13-caption)"
 $ out.width: chr "9.5in"

  |.....................................                                 |  53%
  ordinary text without R code

  |......................................                                |  54%
label: figure14 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig14-caption)"

  |.......................................                               |  55%
  ordinary text without R code

  |.......................................                               |  56%
label: figure15 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig15-caption)"
 $ out.width: chr "5.0in"

  |........................................                              |  57%
  ordinary text without R code

  |.........................................                             |  58%
label: figure16 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig16-caption)"

  |..........................................                            |  60%
  ordinary text without R code

  |..........................................                            |  61%
label: figure17 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig17-caption)"
 $ out.width: chr "7.5in"

  |...........................................                           |  62%
  ordinary text without R code

  |............................................                          |  63%
label: figure18 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig18-caption)"

  |.............................................                         |  64%
  ordinary text without R code

  |..............................................                        |  65%
label: figure19 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig19-caption)"

  |..............................................                        |  66%
  ordinary text without R code

  |...............................................                       |  67%
label: figure20 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig20-caption)"

  |................................................                      |  69%
  ordinary text without R code

  |.................................................                     |  70%
label: figure21 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig21-caption)"

  |..................................................                    |  71%
  ordinary text without R code

  |..................................................                    |  72%
label: figure22 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig22-caption)"
 $ out.width: chr "6.5in"

  |...................................................                   |  73%
  ordinary text without R code

  |....................................................                  |  74%
label: figure23 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig23-caption)"
 $ out.width: chr "7.5in"

  |.....................................................                 |  75%
  ordinary text without R code

  |.....................................................                 |  76%
label: figure24 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig24-caption)"

  |......................................................                |  78%
  ordinary text without R code

  |.......................................................               |  79%
label: figure25 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig25-caption)"

  |........................................................              |  80%
  ordinary text without R code

  |.........................................................             |  81%
label: figure26 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig26-caption)"

  |.........................................................             |  82%
  ordinary text without R code

  |..........................................................            |  83%
label: figure27 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig27-caption)"

  |...........................................................           |  84%
  ordinary text without R code

  |............................................................          |  85%
label: figure28 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig28-caption)"

  |.............................................................         |  87%
  ordinary text without R code

  |.............................................................         |  88%
label: figure29 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig29-caption)"

  |..............................................................        |  89%
  ordinary text without R code

  |...............................................................       |  90%
label: figure30 (with options) 
List of 2
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig30-caption)"

  |................................................................      |  91%
  ordinary text without R code

  |................................................................      |  92%
label: figure31 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig31-caption)"
 $ out.width: chr "5.5in"

  |.................................................................     |  93%
  ordinary text without R code

  |..................................................................    |  94%
label: figure32 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig32-caption)"
 $ out.width: chr "5.5in"

  |...................................................................   |  96%
  ordinary text without R code

  |....................................................................  |  97%
label: figure33 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig33-caption)"
 $ out.width: chr "7.5in"

  |....................................................................  |  98%
  ordinary text without R code

  |..................................................................... |  99%
label: figure34 (with options) 
List of 3
 $ fig.align: chr "center"
 $ fig.cap  : chr "(ref:fig34-caption)"
 $ out.width: chr "4.75in"

  |......................................................................| 100%
  ordinary text without R code


output file: techreport.knit.md

"C:/Users/JacksonJ/ANACON~1/Scripts/pandoc" +RTS -K512m -RTS techreport.knit.md --to latex --from markdown+autolink_bare_uris+tex_math_single_backslash --output techreport.tex --lua-filter "C:\Users\JacksonJ\AppData\Local\renv\cache\v5\R-4.0\x86_64-w64-mingw32\bookdown\0.22\8fb0b67dfdf9d751fc93cb0036def3cc\bookdown\rmarkdown\lua\custom-environment.lua" --lua-filter "C:\Users\JacksonJ\AppData\Local\renv\cache\v5\R-4.0\x86_64-w64-mingw32\rmarkdown\2.8\f518ba47713f92d0d603eec7c6888faf\rmarkdown\rmarkdown\lua\pagebreak.lua" --lua-filter "C:\Users\JacksonJ\AppData\Local\renv\cache\v5\R-4.0\x86_64-w64-mingw32\rmarkdown\2.8\f518ba47713f92d0d603eec7c6888faf\rmarkdown\rmarkdown\lua\latex-div.lua" --metadata-file "C:\Users\JacksonJ\AppData\Local\Temp\1\RtmpgBRgxg\file54bc2460464f" --self-contained --table-of-contents --toc-depth 2 --template "C:\Users\JacksonJ\AppData\Local\renv\cache\v5\R-4.0\x86_64-w64-mingw32\csasdown\0.0.10.9000\57f2cf63e839d922570cf1fe22c1f26b\csasdown\csas-tex\tech-report.tex" --number-sections --highlight-style tango --pdf-engine xelatex --top-level-division=chapter --wrap=none --default-image-extension=png --include-in-header "C:\Users\JacksonJ\AppData\Local\Temp\1\RtmpgBRgxg\rmarkdown-str54bc371224f.html" --variable tables=yes --standalone -Mhas-frontmatter=false --citeproc 
[1] "C:/DEV/new-gully-report/_book/techreport.pdf"

Output created: _book/techreport.pdf
Warning messages:
1: In eval(expr, envir, enclos) : NAs introduced by coercion
2: LaTeX Warning: Command \underbar  has changed.
               Check if current package is valid.
LaTeX Warning: Command \underline  has changed.
               Check if current package is valid. 
```

Once the document is knit together a PDF file (***techreport.pdf***)
will appear in the viewer. The PDF will also be stored in the \_book
folder that is generated upon execution of the **Knit** function.

</br>

## Customization of csasdown Files for the Gully Technical Report

A number of additional R packages were added to the
GullyReproducibleReport.Rproj at various stages to allow certain
functionality, such as adding special characters and symbols not
accessible in base R:

1.  Added LaTeX package **xfrac** so that I could add the code for
    creating the ¾ symbol.

2.  Added LaTeX package **gensymb** so that I could use the code for the
    degree symbol.

3.  Added LaTeX package **multirow** so that I could collapse more than
    one row using the kableExtra function collapse_rows.

4.  Added LaTeX packages **colortbl** and **xcolor** to use colors in
    tables using the “striped” or “background” kable options.

5.  Added LaTeX package **bookmark** to allow more options when
    configuring bookmarks (cross-reference commands to create hypertext
    links).

6.  Added LaTeX package **mathptmx** which gives mathematical
    expressions the same or similar font as the normal text.

</br>

It is also required to download and install the free version of
[Ghostscript](https://www.ghostscript.com).

The path (e.g. *(C:\\Program Files\\gs\\gs9.54.0\\bin)*) to the
Ghostscript executable must be added to the Windows user PATH variable.

One of the authors created the report in Microsoft Word. The text was
copied and pasted into the various RMarkdown files. Some of the symbols
that were in the Word document had to be replaced with their LaTeX
equivalents for them to be output correctly in the PDF document; for
example:

``` r
plus/minus symbol: ± changed to $\pm$
Three-quarters symbol: ¾ change to $\textthreequarters$
Greek letter Mu: µ changed to $\mu$
Degree symbol: ⁰ changed to $\degree$
```

Some other useful features that were discovered during the generation of
this report were:

To make a subscript 2 use the following type of format to do it:
\\textsubscript{2}

To make a superscript th use the following type of format to do it:
\\textsuperscript{th}

To make it so that some text strings in a data frame are italicized in
the output table, I used the LaTeX command: \\textit{}

For example: zoopl \<- c("LL\\\_07/Fall“,”Year“,”\\textit{C.
finmarchicus}“,”\\textit{Clausocalanus}“,”\\textit{Microcalanus}“,”GULD\\\_03/Fall“,”Year“,”\\textit{Clausocalanus}“,”\\textit{M.
lucens}“,”HL\\\_06/Fall“,”Year“,”\\textit{C. finmarchicus}“,”\\textit{M.
clausi}“,”\\textit{M. lucens}“,”\\textit{O. atlantica}“,”\\textit{O.
similis}“,”\\textit{Paracalanus}“,”\\textit{Pleuromamma}")

There were horizontal lines in the header and footer of landscape pages.
To remove these from the PDF document, I deleted the two identical lines
(seen below) from the techreport.sty file.

{\\rotatebox{90}{\\rule{9in}{0.25pt}}}\\end{textblock}

Note that the csasdown package creators may change things so that this
is removed already so it might not be an issue in future report
projects.

</br>

## Required edits to the LaTeX intermediate file (techreport.tex)

There are two formatting issues that need to be fixed but it involves the editing of the intermediate [LaTeX](https://www.latex-project.org/) file (**_techreport.tex_**) that was generated from the **RMarkdown** files. It is stored in the **\_book** sub-folder where the **_techreport.pdf_** file is also located. The LaTeX file is used to generate the PDF document.

Before editing the LaTeX file it must be copied from the **\_book** sub-folder to the main folder.

### Problems to Fix

**Problem 1:** The TABLES and FIGURES are subsections of the REFERENCES section instead of being their own sections.

**Solution:** Removed the "sub" prefix from the TABLES and FIGURES sections.

```latex
\hypertarget{sec:tables}{%
\subsection{TABLES}\label{sec:tables}}

became

\hypertarget{sec:tables}{%
\section{TABLES}\label{sec:tables}}

and

\hypertarget{sec:figures}{%
\subsection{FIGURES}\label{sec:figures}}

became

\hypertarget{sec:figures}{%
\section{FIGURES}\label{sec:figures}}
```
<br/>

**Problem 2:** Inline references when there are more than two authors are indicated with the format **\<Primary Author\> et al.**. Normally this **_et al._** phrase, which is the abbreviated version of the Latin phrase "et alia" meaning "and others", is italicized. I could not achieve this in RMarkdown.

**Solution:** Did a search in the LaTeX document to find and replace all occurrences of "et al." with "\textit{et al.}". All 51 occurrences were changed.

## Generate new PDF document

Since the intermediate LaTeX file was edited it has to be converted to PDF format outside of RStudio.

This is done within an operating system console window (using Windows 10 Anaconda command prompt in this instance but you can use the Terminal window within RStudio just as easily) by executing the following command:

(base) PS C:\\dev\\new-gully-report> **xelatex techreport.tex**

One will notice at first glance that:

- the resulting PDF document has the correct section numbers for the Tables and Figures, 
- the "et al." phrases are all italicized, but
- the Table of Contents (TOC) is totally empty, and
- some in-document links to the tables and figures may be invalid as indicated by question marks.

Checking the console window one will notice two warnings at the end of compilation, which are: 

1. LaTeX Warning: There were undefined references.

2. LaTeX Warning: Label(s) may have changed. Rerun to get cross-references right.

Re-running the command will generate the TOC and fix the in-document links to tables and figures but still one of the two warnings remained.  Executing the command a third time gets rid of the last warning.

## Congratulations! The final report has been generated. :smiley:
