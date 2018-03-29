---
title: 'Generate a word cloud in R'
# caption_url: ''
image: /img/man-in-arena.png
comments: true
---
As a way of learning new ideas and skills I resolved to taking little experiments.

An interesting lesson is generating a word-cloud in R  <!--more-->
with [RStudio][1] .

 In order toTo get started generating a word cloud,

 we need to install the following packages :

```R
    install.packages("tm") #for textmining
    install.packages("SnowballC") #for text stemming
    install.packages("wordcloud") #word cloud generator
    install.packages("RColorBrewer") # import color palettes
```

Load up the above installed packages with ;
~~~ R
    library("tm")
    library("SnowballC")
    library("wordcloud")
    library("RColorBrewer")
~~~
I downloaded Theodore Roosevelt's, [Man in the Arena][2] speech
and saved it in a text file.
To access it the file in R, locate the text file and read its contents.
~~~R     
    #read file
    filePath <- "/home/wordcloud/man_in_arena.txt"
    text <- readLines(filePath)

    #load data
    docs <- Corpus(VectorSource(text))

    #inspect contents of the document
    inspect(docs)
~~~

Modify the contents within the document that would improve semantics; reduce
noise within the text and factors such as white spaces
~~~R
    #replace "/, @, |" with space
    toSpace <- content_transformer(function(x, pattern) gsub(pattern, " ", x))

    #remove unnecessary whitespaces
    docs <- tm_map(docs, toSpace, "@")
    docs <- tm_map(docs, toSpace, "\\|")
    docs <- tm_map(docs, toSpace, "/")
~~~
### [Text stemming][3]
Stemming is the process of reducing inflected words to their word stem
As an example `fishing` has its root word as `fish`

To exercise this, you can remove words that you'd like to;
~~~R
    #remove common words(stop words) &
    #convert to lowercase
    docs <- tm_map(docs, content_transformer(tolower))
    #remove numbers
    docs <- tm_map(docs, removeNumbers)
    #remove english common stop words
    docs <- tm_map(docs, removeWords, stopwords("english"))
    #remove own / undesired stopword
    docs <- tm_map(docs, removeWords, c("Theodore", "We", "Shall"))
    #remove punctuations
    docs <- tm_map(docs, removePunctuation)
    #eliminate extra white spaces
    docs <- tm_map(docs, stripWhitespace)
     #text stemming
    docs <- tm_map(docs, stemDocument)
~~~

### Build a term-document matrix
As a reference, this allows you to index each word in the
text file and the frequency it appears in the document.
~~~R
    dtm <- TermDocumentMatrix(docs)
    m <- as.matrix(dtm)
    v <- sort(rowSums(m), decreasing = TRUE)
    d <- data.frame(word = names(v), freq=v)
    head(d, 10)
    #sample output
        word freq
        great    3
        actual   2
        deed     2
        fail     2
        strive   2
        achieve  1
        arena    1
~~~
and finally,
### Generate the word cloud with :
~~~R
    set.seed(1200)
    wordcloud(words = d$word, freq = d$freq, min.freq = 1,
              max.words = 200, random.orders=FALSE, rot.per=0.35,
              colors=brewer.pal(9, "RdPu"))
~~~

<!-- <img src="https://github.com/4bic/4bic_website/blob/master/images/man-in-arena.png?raw=true" width="650"> -->


<!-- Hyperlinks in the text -->

[1]:rstudio.com
[2]:http://www.artofmanliness.com/2009/02/28/manvotional-the-man-in-the-arena-by-theodore-roosevelt/
[3]:https://en.wikipedia.org/wiki/Stemming
