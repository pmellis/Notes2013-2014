###Coding Advocacy: Visualizing Supreme Court Arguments and Formality

Recently, Professor Josh Blackman generously entertained a question of mine regarding semantic analysis of Supreme Court oral arguments. I’m especially interested in how the justices’ questions correlate, if at all, to the way they vote and a case’s outcome. Following this conversation, I looked into the question a bit more and some creative ways of studying oral argument. I found [this fantastic post](http://trinkerrstuff.wordpress.com/2012/10/04/presidential-debates-with-qdap-beta/) from the Tyler Rinker’s blog demonstrating Rinker’s R package [qdap’s](http://cran.r-project.org/web/packages/qdap/qdap.pdf) power and flexibility for dialogue analysis. I thought I’d give it a try on a Supreme Court oral argument.

####Getting and Cleaning Transcript

I performed this analysis on a recent favorite: Koontz v. St. John’s River Water Management. The transcript is available here. First, we need to convert the .pdf into a machine-readable format (.docx, .txt, etc.). I did it manually, i.e. copy and pasting. This is time-consuming and I would love to find a way to automate this, but no luck on that yet.

Once we’ve accomplished that, we can upload it to R and clean the text:

```{r}
library(qdap)
dat <- read.transcript("TRANSCRIPT.DOCX", col.names=c("person", "dialogue"))
truncdf(dat)
left.just(dat)
# qprep wrapper for several lower level qdap functions
# removes brackets & dashes; replaces numbers, symbols & abbreviations
dat$dialogue <- qprep(dat$dialogue)  
# sentSplit splits turns of talk into sentences
dat2 <- sentSplit(dat, "dialogue", stem.col=FALSE)  
htruncdf(dat2)   #view a truncated version of the data(see also truncdf)
```

This should produce a preview of the cleaned transcript:

```
person tot   dialogue
1  BEARD 1.1 Thank you,
2  GINSBURG 2.1 Let's back
3  GINSBURG 2.2 When he as
4  GINSBURG 2.3 So he reco
5  GINSBURG 2.4 Is that ri
6  BEARD 3.1 That is co
7  BEARD 3.2 With his a
8  GINSBURG 4.1 And if he 
9  BEARD 5.1 If there w
10 GINSBURG 6.1 Suppose he
```

####Gantt Plot of Argument

Next, we can visualize the argument with the following:

```{r}
with(dat2, gantt_plot(dialogue, person, title = "Koontz v. St. John's Argument Viz",  xlab = "Argument Duration", ylab = "Speaker", x.tick=TRUE,
+ minor.line.freq = NULL, major.line.freq = NULL, rm.horiz.lines = FALSE))
```

![pic](http://patellis.files.wordpress.com/2014/04/rplot6.png?w=788&h=174)

Note Justice Breyer’s soliloquy around 2000 words . . .

####Formality Scores

And finally, if you want to go a little deeper on the analysis, we can visualize the formality of speech:

```{r}
#parallel about 1:20 on 8 GB ram 8 core i7 machine
v1 <- with(dat2, formality(dialogue, person, parallel=TRUE))
plot(v1)
#about 4 minutes on 8GB ram i7 machine
v2 <- with(dat2, formality(dialogue, person)) 
plot(v2)
# note you can resupply the output from formality back
# to formality and change arguments.  This avoids the need for
# openNLP, saving time.
v3 <- with(dat2, formality(v1, person))
plot(v3, bar.colors=c("Dark2"))
```

![pic](http://patellis.files.wordpress.com/2014/04/rplot013.png?w=785&h=280)

Interestingly, Justice Alito appears to be the the most informal (coolest justice?), while Mr. Kneedler keeps it classy.

