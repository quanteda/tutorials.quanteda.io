---
title: Multiple text files
weight: 20
draft: false
---

Text data does not always come conveniently packaged into a single spreadsheet. Often you will instead have a folder of separate files, one document per file, common when texts were scraped from the web or exported one at a time from another system. You can also load multiple text files like these at once, from a single folder or from a set of subfolders. Again, `path_data` is the location of sample files that come bundled with the **readtext** package.


``` r
library(quanteda)
library(readtext)
```


``` r
path_data <- system.file("extdata/", package = "readtext")
```

Unlike pre-formatted spreadsheet files, individual text files usually carry no document-level variables of their own, since a plain `.txt` file is just the text and nothing else. The **readtext** package works around this by letting you extract document-level variables from the file names themselves, provided the file names follow a consistent pattern.

The directory `/txt/UDHR` contains text files (".txt") of the Universal Declaration of Human Rights in 13 languages.

The asterisk (`*`) at the end of the path tells `readtext()` to read every file in that folder, rather than one specific file by name.


``` r
dat_udhr <- readtext(paste0(path_data, "/txt/UDHR/*"))
```

{{% notice note %}}
If you are using Windows, you might need to specify the encoding of the file by adding `encoding = "utf-8"`. In this case, imported texts might appear like `<U+4E16><U+754C><U+4EBA><U+6743>` but they indicate that Unicode characters are imported correctly, and they will display properly once you use them in an analysis.
{{% /notice %}}

You can generate document-level variables based on the file names using the `docvarnames` and `docvarsfrom` arguments, especially useful when your files are named in a structured way, for example `unit_context_year_language_party.txt`, since each part of the name can become its own variable. `dvsep = "_"` specifies the character that separates these values in the filenames, and `encoding = "ISO-8859-1"` tells **readtext** which character encoding the text files themselves use.


``` r
dat_eu <- readtext(paste0(path_data, "/txt/EU_manifestos/*.txt"),
                    docvarsfrom = "filenames", 
                    docvarnames = c("unit", "context", "year", "language", "party"),
                    dvsep = "_", 
                    encoding = "ISO-8859-1")
str(dat_eu)
```

```
## Classes 'readtext' and 'data.frame':	17 obs. of  7 variables:
##  $ doc_id  : chr  "EU_euro_2004_de_PSE.txt" "EU_euro_2004_de_V.txt" "EU_euro_2004_en_PSE.txt" "EU_euro_2004_en_V.txt" ...
##  $ text    : chr  "PES · PSE · SPE European Parliament rue Wiertz B 1047 Brussels\n\nGEMEINSAM WERDEN WIR STÄRKER Fünf Verpflichtu"| __truncated__ "Gemeinsames Manifest\nGemeinsames Manifest zur Europawahl 2004 Europäischen Föderation Grüner Parteien (EFGP) \"| __truncated__ "PES · PSE · SPE European Parliament rue Wiertz B 1047 Brussels\n\nGROWING STRONGER TOGETHER Five commitments fo"| __truncated__ "Manifesto\nEuropean Elections Manifesto 2004\nCOMMON PREAMBLE\nAs adopted at 15th EFGP Council, Luxembourg, 8th"| __truncated__ ...
##  $ unit    : chr  "EU" "EU" "EU" "EU" ...
##  $ context : chr  "euro" "euro" "euro" "euro" ...
##  $ year    : int  2004 2004 2004 2004 2004 2004 2004 2004 2004 2004 ...
##  $ language: chr  "de" "de" "en" "en" ...
##  $ party   : chr  "PSE" "V" "PSE" "V" ...
```

### JSON

Social media data is often distributed as JSON, a structured text format that stores nested key-value pairs rather than plain rows and columns. **readtext** can read this format too. [twitter.json](https://raw.githubusercontent.com/quanteda/tutorials.quanteda.io/master/content/data/twitter.json) is located in the data directory of this tutorial package, and it is a small sample of tweets saved in this format. (Collecting new data this way is no longer possible: Twitter/X's public API has changed substantially since this sample was collected. Reading an existing JSON file like this one still works the same way, regardless of where it came from.)


``` r
dat_twitter <- readtext("../data/twitter.json", source = "twitter")
```

Setting `source = "twitter"` tells **readtext** to expect the specific structure of tweet data, automatically pulling out metadata fields that come with each tweet, such as the number of retweets and likes, the username, time and time zone.


``` r
head(names(dat_twitter))
```

```
## [1] "doc_id"         "text"           "retweet_count"  "favorite_count"
## [5] "favorited"      "truncated"
```

### PDF

`readtext()` can also convert and read PDF (".pdf") files directly, extracting the underlying text so you do not have to copy and paste it by hand.


``` r
dat_udhr <- readtext(paste0(path_data, "/pdf/UDHR/*.pdf"),
                      docvarsfrom = "filenames",
                      docvarnames = c("document", "language"),
                      sep = "_")
```

### Microsoft Word

Finally, `readtext()` can import Microsoft Word (".doc" and ".docx") files in the same way, useful if your texts were originally typed up or shared as Word documents rather than plain text.


``` r
dat_word <- readtext(paste0(path_data, "/word/*.docx"))
```

Whichever format your texts started in, `.csv`, individual `.txt` files, JSON, PDF or Word, the result of `readtext()` is always the same kind of data frame, ready to be passed to `corpus()` in the same way.
