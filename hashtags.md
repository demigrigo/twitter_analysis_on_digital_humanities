# In the world of hashtags
Hashtags are considered to be a form of labelling which is related to the core meaning/main attribute of the post published online.  Twitter was one of the innovative social media which firstly introduced #hashtag to the world back in 2006. Now 12 years later #hashtag has evolved into a very useful tool for political analytics, advertisement companies and other forms of analysis. Implementing this new trend into our project we would like to find out what people say about #elexis_eu and #digitalhumanities or what the aforementioned concepts are about.
In order to retrieve the appropriate results for this analysis two tools were put in use, namely Twitter and R statistical program. The first one is the source of our data and the second one is the program which will be employed with the aim of analyzing the data. The most important thing for this project is the creation of a code which will be accurate enough to acquire the relevant results. To do so, firstly, we have to install the needed packages (viz. Twitter, wordcloud and text mining packages) into R and then, to set up our account for our project. Consider the following:
```install.packages("twitteR")
install.packages("wordcloud")
install.packages("tm")

library("twitteR")
library("wordcloud")
library("tm")
download.file(url="http://curl.haxx.se/ca/cacert.pem", destfile="cacert.pem")
consumer_key <- 'xxxxxx'
consumer_secret<- 'xxxxxxx'
access_token <- 'xxxxxx'
access_secret <- 'xxxxxx'
setup_twitter_oauth(consumer_key,consumer_secret, access_token, access_secret)
```
 Having installed and set up our Twitter account in R, the search of our first hashtag, namely #elexis_eu takes place. For this reason, the following code is necessary for R:
```
r_elexis <- searchTwitter("#elexis_eu", n=3000)
r_elexis_text <- sapply(r_elexis, function(x) x$getText())
```
The first line represents the search in Twitter for the #elexis_eu and the number of tweets that it is needed for this project. For this hashtag the maximum amount of tweets is needed and thus, 3000 was considered suitable. The second line of this code transforms our data into a text. Despite having assigned 3000 entrances of the research, the system offers only 50 tweets available.  The next step of this procedure is the creation of the corpus where deeper investigation takes place.  In order to achieve it, the following code is put in use:
```
r_elexis_text_corpus <- Corpus(VectorSource(r_elexis_text))
r_elexis_text_corpus<- tm_map(r_elexis_text_corpus, content_transformer(function(x) iconv(x, to='UTF-8-MAC', sub='byte'))

```
Additionally, before requesting the final wordcloud it is better to make some adjustments to the text (e.g. transform the text into  lowercase) as well as remove some words. For this reason, the following codes are considered useful:
```
r_elexis_text_corpus <- tm_map(r_elexis_text_corpus, content_transformer(tolower)) 
r_elexis_text_corpus <- tm_map(r_elexis_text_corpus, removePunctuation)
r_elexis_text_corpus = tm_map(r_elexis_text_corpus, trimws)
r_elexis_text_corpus <- tm_map(r_elexis_text_corpus, stripWhitespace)
r_elexis_text_corpus <- tm_map(r_elexis_text_corpus, function(x)removeWords(x,stopwords())
```

Having cleaned up the unnecessary information from the corpus, it is time to create the wordcloud. The following code is an example of how the final wordcloud has been constructed:
```
install.packages("RColorBrewer")
library(RColorBrewer)
pal2 <- brewer.pal(8,"Dark2")
wordcloud(r_elexis_text_corpus,min.freq=2,max.words=100, random.order=T, colors='pal2')
```
![image alt text](https://github.com/demigrigo/twitter_analysis_on_digital_humanities/blob/master/elexis.png)
   ### Fig.1 Wordcloud of #elexis_eu (retrieved 29.07.2018)

According to Figure 1, the examined hashtag is related to the concepts of *lexicography*, *clarineric* along with the general field of *research*. Based on the aforementioned wordcloud the concept of “elexis” can be explained or understood easier simply by observing the words written in bigger letters. Since the project is new in the world of Twitter and consequently, in the world of hashtags we assume that additional information/words will be added as the project continues to grow. 
The concept of #elexis_eu is associated with digital humanities, which is a fast growing domain within the academia. For this reason, it would also be interesting to investigate the words related to this field as well as the general concepts surrounding it. Applying the same codes into the hashtag of *digitalhumanities*, the following wordcloud is created:

![image alt text](https://github.com/demigrigo/twitter_analysis_on_digital_humanities/blob/master/digital%20humanities%20better.png)
### Fig.2. Wordcloud of #digitalhumanities (retrieved 09.08.2018)

The aforementioned wordcloud suggests that #digitalhumanities is a popular topic among the internet community of Twitter. In particular, it can be found out that this hashtag is related to the field of *research, data handling* and *data mining*. It can also be seen that in the center of the schema the words-phrases, such as  *digital humanities, now, registrations* are the ones which are immediately evident, which could suggest that these are the main topic of interest of the twitter users. Concepts such as programming, models, annotation etc., on the other hand, can be spotted with smaller letters away from the circle’s core. This could indicate that users do not often concern themselves with these concepts and therefore are used less frequently. Having this information about the most frequent words in this hashtag  it would be interesting to discover which are these exact words.

![image alt text](https://github.com/demigrigo/twitter_analysis_on_digital_humanities/blob/master/most%20frequent%20words.png)

### Fig.3. Most frequent terms of #digitalhumanities in 2000 tweets (retrieved 09.08.2018)

Figure 3 presents the terms used in Twitter with frequency >=90. The most frequent word/phrase is the hashtag itself  (i.e. digitalhumanities) with 800 instances in the sample of 2000 tweets. Following this phrase the words *digital* and *humanities* appear with approximately 350 and 225 appearances in the course of 2000 tweets, respectively. According to the diagram the words *research* and *dh*  are less frequent in the examined sample.
Concerning the hashtag of digital humanities more information can be retrieved due to the vast amount of Tweets. One example could be to research the word associations of some terms related to the field of digital humanities. To do so, the following code can be implemented:
```
findAssocs(tdm, "research", 0.2)
```

The aforementioned code can be used in order to detect the word associations within a term matrix. Inside the brackets, the first element represents the data set (i.e. term matrix), the second one the word or phrase that we are interested in and the last one indicates the correlation limit. Applying the code for the word “research” we receive these results:

```
$`research`
germany      kevinbgunn              math       brno
  0.35        0.32                    0.21       0.21

```

The above results show that the highest and the lowest correlation scores of the word “research” based on our data. To begin with, the word “research” is highly associated with the country of Germany by 0.35. It has also strong correlation with the researcher Kevin B. Gunn. The lowest scores, on the other hand, can be found in terms such as “math” and “brno”. Of course, the list offered in R is much longer and it is worth investigating even further.
In similar way, we researched for the terms “data” and “dh” (i.e. abbreviated form for digital humanities). Here are some of the highest and lowest results for each of the terms:

```
$`data`
britishlibrary     journalism         miaout
0.80               0.80               0.80
acledinfo            armed
0.22                0.22

$`dh`
chen                jclc              jing
0.42                0.42              0.42
quantifying        mickikaufman       mappproject
0.23                0.23              0.21

```

As far as the first results are concerned, it can be said that “data” are strongly with the British library and journalism It is also interesting to see that “miaout” is strongly correlated with the concept of data, as this term correspond to the username of Dr Mia Ridge who is the digital curator of the British library which offers collections of data. Less frequent are the terms such as “armed”, and “acledinfo” which stands for Armed Conflict Location and Event Data information. Interpreting the results of word correlations of the acronym “dh” (i.e. digital humanities), it can be pointed out that Jing Chen is strongly related to this concept, as he recently acquired a PhD on this field, as well as the acronym “jclc” (i.e. Journal of Chinese Literature and Culture) which has some articles concerning digital humanities. Terms namely “quantifying” and “mickikaufman” have proven to be less associated with the concept under investigation.
As final step related to word associations and digital humanities is the visualization of the terms into a wordnet, by applying this code:

```
source("http://bioconductor.org//biocLite.R")
biocLite("Rgraphviz")
install.packages (“Rgraphviz”)
library(“Rgraphviz”)
plot(tdm, terms = freq.terms, corThreshold = 0.1, weighting = T)
```

The following figure illustrates this relation:

![image alt text](https://github.com/demigrigo/twitter_analysis_on_digital_humanities/blob/master/word%20associations%2009082018%20tweets.png)

### Fig.4. Network of terms in relation to  #digitalhumanities (retrieved 09.08.2018)

Figure 4 reveals the word associations and possible collocations that users have typed in their tweets. In particular, it can be  noted that the nodes written in bold represent the strongest correlations among words such as “historical job”, “dariahteach and course”. The nodes that are less bold indicate “weak” associations, but still strong enough to evident on the network. In this sense, it can be understood that “r” (presumably the computer program) is related both to “science” as well as to “open”.
In the world of hashtags there is another trend in the field of research, namely the sentiment analysis. An attempt will be made to detect the content of the tweets concerning #digitalhumanities and to classify it into positive, negative or neutral, depending on the content of the tweets.  By applying this code, the appropriate results are obtained:


```
require(devtools)
install_github("sentiment140", "okugami79")
library(sentiment)
sentiments <- sentiment(tweets.df$text)
table(sentiments$polarity)
```

The results retrieved from the aforementioned code show the following: 

```
negative  neutral positive 
       2     1341      657
```

The result indicate that there are 2 negative tweets about digital humanities, 1341 are the negative tweets, whereas 657 are the positive ones. Plotting these tweets into a graph will be appeared as it follows:

![image alt text](https://github.com/demigrigo/twitter_analysis_on_digital_humanities/blob/master/sentiment%20analysis.png)

### Fig.5. Sentiment analysis for #digitalhumanities (retrieved 09.08.2018) 

Figure 5 illustrates the scores of sentiment analysis for the 2000 tweets provided by Twitter for the time period 30 July-09 August. The y axis indicates the score of the analysis from 0-100, and the x axis illustrates the date that the tweets are published. According to the graph above on 5th August the line dramatically falls, which could possibly suggest the appearance of the negative tweets.  Asking R for the polarity scores of the sample, it is possible for us to extract the negative tweets from the sample. By doing so, we receive the two negative tweets:

*“todays #followwomenwednesday =people whose work I've been looking at today as I try to find #womenshistory in #digitalhumanities @xxxxxx @xxxxxx @xxxxx @xxxxx @xxxxx (lots don't do twitter apparently!)”*

The above-mentioned tweet has been retweeted twice in the sample and is considered negative since it includes the phrase *don’t*  which is a marker of negation. 

All in all it can be concluded that, Twitter is a useful tool in the field of research. Researchers can apply the aforementioned codes in order to retrieve information about the current trends. We hope that in the future Twitter will allow its users to investigate data which have been published longer than one week ago. Thus, a diachronic analysis of tweets from different periods of time might be conducted. 





By Dimitra Grigoriou. Current work was a part of an internship at the ACDH.
