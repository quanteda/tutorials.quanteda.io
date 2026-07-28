---
title: Convolutioanl Neural Network
weight: 90
draft: false
---



Convolutioanl neural network (CNN) is a type of supervised machine larning tecnique that represents cooccrances of tokens in the neibourhood. The model captures similar information as n-grams and skip-grams but does not require manual feature engineering. If CNN is trained on a sizable annotated corpus, it can predict the labels accurately. 

**quanteda** works as an NLP infrastructure for **torch** and **luz**, which implements more general neural network models. We can install these packages from CRAN.


``` r
install.packages("torch")
install.packages("luz")
```


``` r
library(quanteda)
library(quanteda.corpora)
library(torch)
library(luz)
quanteda_options(verbose = TRUE)
```

## Download example data

Download the corpus of movie review texts with sentiment annotation using **quanteda.corpora**'s `download()` function. See the [package website](https://quanteda.io/articles/pkgdown/examples/neural-networks.html#construct-corpus) for how the corpus was created.


``` r
corp_imdb <- download(url = "https://www.dropbox.com/scl/fi/mb88hqwfly7t3u30fks4q/data_corpus_imdb.RDS?rlkey=hlzie0qdumkwexnp7bz8539ng&st=owqlekqm&dl=1")
```



## Tokenize texts

We tokenize texts using `tokens()`, remove only punctuations, and lower-case them using `tokens_tolower()`. To limit the complexity of the data, we apply `tokens_trim()` with the new `max_n` argument. This limits the size of the vocabulary (unique token types) to the top 20,000 most frequent words in the object.


``` r
vocab_size <- 20000 # maximum size of the vocabulary

toks_imdb <- tokens(corp_imdb, remove_punct = TRUE) |>
  tokens_tolower() |>
  tokens_trim(max_n = vocab_size)
## Creating a tokens from a corpus object...
##  ...starting tokenization
##  ...tokenizing 1 of 1 blocks
##  ...preserving hyphens
##  ...preserving elisions
##  ...preserving social media tags (#, @)
##  ...removing separators, punctuation
##  ...177,293 unique types
##  ...complete, elapsed time: 10.8 seconds.
## Finished constructing tokens from 50,000 documents
## tokens_tolower() changed from 177,293 types (50,000 documents, 11,454,169 tokens) to 147,281 types (50,000 documents, 11,454,169 tokens)
## tokens_trim() changed from 147,281 types (50,000 documents, 11,454,169 tokens) to 20,000 types (50,000 documents, 11,072,518 tokens)
```

## Define dataset

We use `dataset()` to feed data from the tokens object to neural network models. We can quickly access the documents in the tokens object using `quanteda::as.matrix()` with the `extract` argument. The function returns the documents in the same length: when the documents are longer than `length`, the vectors are truncated; if they are shorter, the vectors are padded by zero. Since the indices in `torch_tensor` is one-based, we shift token IDs by adding one (the ID for padding is zero in tokens objects).

Importantly, before separating the tokens object into training and test sets using `tokens_subset()`, we have to convert it to a  [tokens_xptr](https://quanteda.io/articles/pkgdown/tokens_xptr.html) object for quick access to the documents. 

{{% notice tip %}}
The same word should recieve the same token IDs in training and test sets in training neural network models. One way to achieve this is spliting a tokens object using `tokens_subset()`. Another way is applying `tokens_match()` to separate tokens objects with the same vocarburary vector.
{{% /notice %}}


``` r
text_length <- 200 # maximum number length of the texts

movie_dataset <- dataset(
  name = "movie_dataset",
  
  initialize = function(data, text_length) {
    self$toks <- data
    self$dvars <- docvars(data)
  },
  .getitem = function(i) {
    list(
      x = as.matrix(self$toks, length = text_length, extract = i) + 1L,
      y = self$dvars$sentiment[i]
    )
  },
  .getbatch = function(i) {
    list(
      x = as.matrix(self$toks, length = text_length, extract = i, drop = FALSE) + 1L,
      y = self$dvars$sentiment[i]
      # alternatively
      #x = as.tensor(self$toks, length = text_length, extract = i),
      #y = torch_tensor(self$dvars$sentiment[i])
    )
  },
  .length = function() {
    ndoc(self$toks)
  }
)

xtoks_imdb <- as.tokens_xptr(toks_imdb) # important!
train_ds <- movie_dataset(tokens_subset(xtoks_imdb, split == "train"), text_length)
test_ds <- movie_dataset(tokens_subset(xtoks_imdb, split == "test"), text_length)
```

If you call the dataset with the document index, it returns the token IDs in `x` and the sentiment label in `y`. Documents are padded by `1` to make all of them to be 200 long.


``` r
train_ds[1:2]
## $x
##           [,1]  [,2] [,3] [,4] [,5] [,6] [,7]  [,8] [,9] [,10] [,11] [,12]
## text25001  260    23    9  633   68    6 3739  1233   11     9  2715  1730
## text25002 3104 15661  126   18   37 3427 3820 13074   23    98    54    95
##           [,13] [,14] [,15] [,16] [,17] [,18] [,19] [,20] [,21] [,22] [,23]
## text25001     8   103     9  3570   363   155    58     9    19   362    23
## text25002    10    58   194    23  1442   110    58  1484  2712    18   148
##           [,24] [,25] [,26] [,27] [,28] [,29] [,30] [,31] [,32] [,33] [,34]
## text25001  6109   264     9 14308 11555  1351    58   394   297   116  7267
## text25002    58   509    18   129    58   509  4469  8600    44    58     9
##           [,35] [,36] [,37] [,38] [,39] [,40] [,41] [,42] [,43] [,44] [,45]
## text25001  3838  5470    53    18  1282    23   133  4518   893   110  7408
## text25002 10011    23   246    43    32     9   460   335    18    10   302
##           [,46] [,47] [,48] [,49] [,50] [,51] [,52] [,53] [,54] [,55] [,56]
## text25001  6109    18   275    54   103    77  5140  2863  1031   498   110
## text25002   217     9  8687  1589    58  3948    41   116  2253    43    58
##           [,57] [,58] [,59] [,60] [,61] [,62] [,63] [,64] [,65] [,66] [,67]
## text25001    30   134    65  5883   464   746    17    18  5395    56    85
## text25002  5588   110   240    41   438  3865    41    18  1065 14754    58
##           [,68] [,69] [,70] [,71] [,72] [,73] [,74] [,75] [,76] [,77] [,78]
## text25001   394    65    18 11222   706   343   390  8838  1060  1609    84
## text25002  1410    41     9  1160   586   145   194   204  2657   794    58
##           [,79] [,80] [,81] [,82] [,83] [,84] [,85] [,86] [,87] [,88] [,89]
## text25001     9  1582  3916    47     9  1717   377   133    71    14   280
## text25002    95   694  2524  2914    23 14652  1586  2648    11   438  3426
##           [,90] [,91] [,92] [,93] [,94] [,95] [,96] [,97] [,98] [,99] [,100]
## text25001   374   611   103   271   273   738    53  3840   543  3840    477
## text25002    89     9    10    58    95  1207   110   146  9505  1586   2648
##           [,101] [,102] [,103] [,104] [,105] [,106] [,107] [,108] [,109] [,110]
## text25001   5349  19309     43  12640  11196    287     85    720  10394      1
## text25002     23   4933     84    596    227   5584    209    155    110     58
##           [,111] [,112] [,113] [,114] [,115] [,116] [,117] [,118] [,119] [,120]
## text25001      1      1      1      1      1      1      1      1      1      1
## text25002      9    681     23     54     43   4322   1723     95     10      1
##           [,121] [,122] [,123] [,124] [,125] [,126] [,127] [,128] [,129] [,130]
## text25001      1      1      1      1      1      1      1      1      1      1
## text25002      1      1      1      1      1      1      1      1      1      1
##           [,131] [,132] [,133] [,134] [,135] [,136] [,137] [,138] [,139] [,140]
## text25001      1      1      1      1      1      1      1      1      1      1
## text25002      1      1      1      1      1      1      1      1      1      1
##           [,141] [,142] [,143] [,144] [,145] [,146] [,147] [,148] [,149] [,150]
## text25001      1      1      1      1      1      1      1      1      1      1
## text25002      1      1      1      1      1      1      1      1      1      1
##           [,151] [,152] [,153] [,154] [,155] [,156] [,157] [,158] [,159] [,160]
## text25001      1      1      1      1      1      1      1      1      1      1
## text25002      1      1      1      1      1      1      1      1      1      1
##           [,161] [,162] [,163] [,164] [,165] [,166] [,167] [,168] [,169] [,170]
## text25001      1      1      1      1      1      1      1      1      1      1
## text25002      1      1      1      1      1      1      1      1      1      1
##           [,171] [,172] [,173] [,174] [,175] [,176] [,177] [,178] [,179] [,180]
## text25001      1      1      1      1      1      1      1      1      1      1
## text25002      1      1      1      1      1      1      1      1      1      1
##           [,181] [,182] [,183] [,184] [,185] [,186] [,187] [,188] [,189] [,190]
## text25001      1      1      1      1      1      1      1      1      1      1
## text25002      1      1      1      1      1      1      1      1      1      1
##           [,191] [,192] [,193] [,194] [,195] [,196] [,197] [,198] [,199] [,200]
## text25001      1      1      1      1      1      1      1      1      1      1
## text25002      1      1      1      1      1      1      1      1      1      1
## 
## $y
## [1] 0 0
```

## Build CNN model

The model starts with a module for word vectors in the embedding layer `nn_embedding`, followed by modules for convolution layers `nn_conv1d()`: the first layer captures the sequence of tokens such as phrases; the second layer further abstracts the occurrences sequences. Finally, the dense feed-forward network `nn_linear()` predicts the sentiment of documents. 


``` r
embedding_dim <- 128 # size of the embedding vectors

model <- nn_module(
    initialize = function(vocab_size, embedding_dim) {
        self$embedding <- nn_sequential(
            nn_embedding(num_embeddings = vocab_size + 1L, embedding_dim = embedding_dim),
            nn_dropout(0.5)
        )
        
        self$convs <- nn_sequential(
            nn_conv1d(embedding_dim, 128, kernel_size = 7, stride = 3, padding = "valid"),
            nn_relu(),
            nn_conv1d(128, 128, kernel_size = 7, stride = 3, padding = "valid"),
            nn_relu(),
            nn_adaptive_max_pool2d(c(128, 1)) # reduces the length dimension
        )
        
        self$classifier <- nn_sequential(
            nn_flatten(),
            nn_linear(128, 128),
            nn_relu(),
            nn_dropout(0.5),
            nn_linear(128, 1)
        )
    },
    forward = function(x) {
        emb <- self$embedding(x)
        out <- emb$transpose(2, 3) |> 
            self$convs() |> 
            self$classifier()
        # we drop the last so we get (B) instead of (B, 1)
        out$squeeze(2)
    }
)

```

## Train the model

We train the hierarchical CNN model on `train_ds`. We only need to define the loss function, learning rate optimizer and evaluation metrics in `setup()`. We set the size of vocaburary and word embedding in `set_hparams()` and specify dataset and the number of iterations in `fit()`. 


``` r
fitted_model <- model |> 
    setup(
        loss = nnf_binary_cross_entropy_with_logits,
        optimizer = optim_adam,
        metrics = luz_metric_binary_accuracy_with_logits()
    ) |> 
    set_hparams(vocab_size = vocab_size, embedding_dim = embedding_dim) |> 
    fit(train_ds, epochs = 3)
```

## Test the model

Evaluate the fitted model `test_ds`. The same evaluation metrics will be used. 


``` r
fitted_model |> 
    evaluate(test_ds) |> 
    print()
## A `luz_module_evaluation`
## ── Results ─────────────────────────────────────────────────────────────────────
## loss: 0.4015
## acc: 0.8201
```

{{% notice tip %}}
We trained word vectors as part of the model using `nn_embedding()` in the example, but we can also use pre-trained word vectors through `nnf_embedding()`. It retrieves word vectors extracted from `wordvector::textmodel_word2vec` with `wordvector::as.matrix(padding = TRUE)`.
{{% /notice %}}

{{% notice ref %}}
- Goldberg, Y. (2017). [Neural network methods for natural language processing. Morgan & Claypool](https://doi.org/10.2200/S00762ED1V01Y201703HLT037). 
- https://skeydan.github.io/Deep-Learning-and-Scientific-Computing-with-R-torch/
- https://mlverse.github.io/luz/articles/examples/text-classification.html
{{% /notice %}}
