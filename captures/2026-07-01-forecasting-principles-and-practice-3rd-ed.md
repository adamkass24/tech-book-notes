# Forecasting: Principles and Practice (3rd ed)
Source: https://otexts.com/fpp3/
Captured: 2026-07-01 | Action: learn

> Forecasting: Principles and Practice (3rd ed.) is a free, continuously-updated online textbook by Hyndman & Athanasopoulos that teaches practical time series forecasting using R's tidyverse-integrated tsibble and fable packages.

## Summary
This is the preface to the 3rd edition of a well-known open-access forecasting textbook aimed at three audiences: business practitioners without formal forecasting training, undergraduate business students, and MBA students taking a forecasting elective. The authors assume only introductory statistics and high-school algebra, flagging the few sections that require matrix knowledge. The book teaches forecasting hands-on using R, requiring the fpp3 package which bundles tidyverse tools (tibble, dplyr, tidyr, lubridate, ggplot2) alongside time-series-specific packages (tsibble, tsibbledata, feasts, fable, ggtime). The most significant change in this edition is the switch from the older 'forecast' package to 'tsibble' and 'fable', enabling tighter tidyverse integration; most examples were rewritten as a result. The authors also reorganized content so that Chapters 2-4 now cover exploratory time series analysis (patterns, features, characteristics) before any forecasting methods are introduced, on the philosophy that you must understand your data before modeling it. New material on time series features was added, and the online version includes short instructional videos at the start of most sections, collected in a YouTube playlist. What distinguishes this book from competitors: it's free, online, continuously updated (unlike print editions that go stale between releases), built entirely on free/open-source R, features dozens of real consulting-derived data examples from the authors' own forecasting engagements with businesses, and places unusually heavy emphasis on graphical methods for exploring data, validating fitted models, and presenting results. Each chapter ends with a 'further reading' list of textbooks or, where none exist, journal articles. The authors solicit typo/error reports via an OTexts discussion forum and correct them online immediately. A citable reference format is given, and the print version remains available via Amazon (last updated 2021) while the online version is updated far more frequently (noted as updated 3 June 2026 at time of access).

## Key Points
- 3rd edition replaced the 'forecast' R package with 'tsibble' and 'fable' for tidyverse-native time series workflows.
- Load the book's tooling via the single 'fpp3' package, which attaches tibble 3.3.1, dplyr 1.2.1, tidyr 1.3.2, lubridate 1.9.5, ggplot2 4.0.3, tsibble 1.2.0, tsibbledata 0.4.1, ggtime 0.2.0, feasts 0.5.0, and fable 0.5.0.
- Chapters 2-4 were reordered to cover exploratory time series analysis (patterns, features) before any forecasting model is introduced.
- Only prerequisites are introductory statistics and high-school algebra; matrix-algebra-dependent sections are explicitly flagged.
- The book targets three distinct audiences: untrained business practitioners, undergraduate business students, and MBA forecasting-elective students.
- fpp3 package loading triggers documented namespace conflicts (e.g., dplyr::filter masks stats::filter, lubridate::date masks base::date) that readers should be aware of when running examples.
- Real-world examples are drawn from the authors' own forecasting consulting work with hundreds of businesses, not synthetic data.
- The online edition is updated continuously to fix errors and add new methods, unlike the static print edition (last updated 31 May 2021, per Amazon).
- Each chapter includes a 'further reading' section pointing to more advanced textbooks or, absent a suitable book, journal articles.
- Short instructional videos accompany most sections online and are aggregated into a YouTube playlist.

## What to Apply
- Install R and run `install.packages('fpp3')` then `library(fpp3)` before working through any examples, matching package versions if reproducing book code.
- Read Chapters 2-4 first for exploratory time series analysis technique before jumping to any forecasting model, per the book's own recommended sequence.
- Bookmark the OTexts discussion forum for troubleshooting R/forecasting questions while working through exercises.
- Check the further-reading list at the end of each chapter when you need deeper theoretical grounding beyond the practical treatment given here.

## What to Skip
- The citation format and print-vs-online version history are administrative details, not content.
- The package namespace-conflict listing is boilerplate console output, not something to memorize.

## Context & Related Topics
- tsibble R package (tidy time series data structure)
- fable R package (tidy forecasting models)
- feasts R package (time series feature extraction and statistics)
- tidyverse ecosystem (dplyr, tidyr, ggplot2, lubridate)
- Hyndman & Athanasopoulos's earlier 'forecast' package, now superseded in this edition

## Books Mentioned (reading list)
- [ ] Forecasting: Principles and Practice, 3rd edition — Rob J Hyndman and George Athanasopoulos
