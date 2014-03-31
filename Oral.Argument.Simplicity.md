###Analyzing the Art of Advocacy: Measuring Simplicity in Oral Argument

####Patrick Ellis

In 2011, Professor Kurt Lash wrote [a short post](http://prawfsblawg.blogs.com/prawfsblawg/2011/05/supreme-court-advocates-the-best-there-ever-was.html) on PrawfsBlawg posing an interesting question:

> How does one measure greatness as a Supreme Court advocate?  Does it involve eloquence?  Most wins?  Best performance on behalf of a worthy cause? Greatest skill in getting the Court to change its mind?

Naturally, an academic, but spirited debate to answer the question ensued in the comments, though no decisively “best” advocate emerged. It’s a highly subjective question and one that I’m not experienced enough to argue about with constitutional law professors. However, one trait of great advocacy neglected in the commentary of Professor Lash’s post is . . . simplicity.

Kanye West and/or Leonardo Da Vinci once [tweeted/wrote](http://news.rapgenius.com/Kanye-west-donda-annotated): Simplicity is the ultimate form of sophistication. A great aphorism for lawyers, though it’s infrequently applied. Simplicity in communication is, of course, essential in any profession: How can we explain a complex idea, product, or service in a way that our target audience will understand? But for attorneys, effectively conveying a complex idea to a court, mediator, or jury may make all the difference in a case’s outcome.

Simplicity in advocacy may not, however, seem as necessary when your audience is the U.S. Supreme Court, a panel of some of the most distinguished and sophisticated legal thinkers of our time. But as Justice John Marshall Harlan II noted in his [1955 address](http://www.appellateinstitute.com/ResourcesPDF/WhatPartDoestheOralArgumentPlay_.pdf) to the Judicial Conference of the Fourth Circuit:

> [I]t seems to me that there are four characteristics which will be found in every effective oral argument, and they are these: first, what I would call “selectivity”; second, what I would designate as “simplicity”; third, “candor”; and fourth, what I would term “resiliency.”

> Simplicity of presentation and expression, you will find, is a characteristic of every effective oral argument. . . . It is sometimes forgotten by a lawyer who is full of his case, that the court comes to it without the background that he has. And it is important to bear this in mind in carrying out the preparation for argument in each of its phases. Otherwise the force of some point which may seem so clear to the lawyer may be lost upon the court.

Even Justice Harlan, one of the most sophisticated and influential jurists of the 20th Century, appreciated simplicity in advocacy. But in the context of Supreme Court advocacy how simple should one be?

In an effort to satisfy this question, I’ve been experimenting a bit with natural language processing and textual analysis tools, courtesy of R. Below is a sample of a larger project I’m working on for Legal Analytics and any feedback will be greatly appreciated. So let’s take a look at an example of how we can measure simplicity in oral advocacy.

Attorney Paul Clement, former Solicitor General, has argued more cases before the Supreme Court than anyone else since 2000. Mr. Clement has been described as [“LeBron James on a fast break”](http://opinionator.blogs.nytimes.com/2012/03/28/moral-arguments/?_php=true&_type=blogs&_r=0) and former Bush attorney general John Ashcroft once referred to him as [“a Michael Jordan-like draft pick.”](http://www.npr.org/2012/03/23/149218361/the-legal-wunderkind-challenging-the-health-law) In other words, Paul Clement is a baller.

Mr. Clement has argued countless, important cases before the Supreme Court, including *McConnell v. FEC, Tennessee v. Lane, Rumsfeld v. Padilla, United States v. Booker, Hamdi v. Rumsfeld,Rumsfeld v. FAIR, Hamdan v. Rumsfeld, Gonzales v. Raich, Gonzales v. Oregon, Gonzales v. Carhart,* and *United States v. Windsor*. Perhaps most famously, or infamously, Mr. Clement also led  the challenge on behalf of 26 states to overturn the [Patient Protection and Affordable Care Act](http://en.wikipedia.org/wiki/Patient_Protection_and_Affordable_Care_Act) in the Supreme Court on March 26 – 28, 2012. We all know how that turned out, but many of Mr. Clement’s arguments were supported by a majority of justices and cannot be fairly characterized simply as a loss. More importantly for my purposes, Mr. Clement, as well as his opponents, did a tremendous job of taking complex arguments and simplifying them . . . at least, I think so. But let’s find out.

Using Mr. Clement’s [March 27, 2012 Individual Mandate argument](http://www.supremecourt.gov/oral_arguments/argument_transcripts/11-398-Tuesday.pdf), we can calculate the “readability,” which I’m using as a proxy for simplicity, of Mr. Clement’s advocacy. (Note: I used this great blog post, [Statistics meets rhetoric: A text analysis of “I Have a Dream”](http://www.r-bloggers.com/statistics-meets-rhetoric-a-text-analysis-of-i-have-a-dream-in-r/) in R by Max Ghenis to do the heavy lifting here). After firing up R, we need to take Mr. Clement’s argument and convert it to raw text. I do this using a website called [textuploader.com](http://textuploader.com/), which allows text be be easily stored and converted to more usable forms. It’s also worth noting that I included the Justices’ questions in the sample. Though this may skew the results, I think it will only have a marginal effect. We can then upload the raw text into R using the following function:

```{r}
speech.raw <- paste(scan(url("Insert textuploader file URL"),
 what="character"), collapse=" ")
```

Now that we have our data in R, we can begin to clean and quantify the text. This can be accomplished by using the qdap package (text analysis) and the data.table package to organize and provide structure to the data:

```{r}
library(qdap)
library(data.table)
```

Next, we need to split the data into sentences, clean the sentences, and count them.

```{r}
argument.df sentences sentences[, sentence.num := seq(nrow(sentences))]
sentences[, person := NULL]
sentences[, tot := NULL]
setcolorder(sentences, c("sentence.num", "speech"))
```

We then calculate the syllables per sentence and find the total syllables to determine where fluctuations in readability occur within the argument.

```{r}
sentences[, syllables := syllable.sum(speech)]
sentences[, syllables.cumsum := cumsum(syllables)]
sentences[, pct.complete := syllables.cumsum / sum(sentences$syllables)]
sentences[, pct.complete.100 := pct.complete * 100]
```

Now, we can use a function to calculate the “readability” of Mr. Clement’s argument based on the Automated Readability Index, a readability test designed to gauge the understandability of a text by producing an approximate representation of the U.S. grade level needed to comprehend the text.

```{r}
sentences[, readability := automated_readability_index(speech, sentence.num)
$Automated_Readability_Index]
```

We almost have all the pieces in place to visualize the readability of Mr. Clement’s argument. We finally need to load visualization packages and define the parameters of our graph.

```{r}
library(ggplot2)
library(scales)
 
my.theme <-
+ theme(plot.background = element_blank(), # Remove background
+ panel.grid.major = element_blank(), # Remove gridlines
+ panel.grid.minor = element_blank(), # Remove more gridlines
+ panel.border = element_blank(), # Remove border
+ panel.background = element_blank(), # Remove more background
+ axis.ticks = element_blank(), # Remove axis ticks
+ axis.text=element_text(size=14), # Enlarge axis text font
+ axis.title=element_text(size=16), # Enlarge axis title font
+ plot.title=element_text(size=24, hjust=0)) # Enlarge, left-align title
 
Plot + return(gg + geom_point(color="grey70") + # Lighten dots
+ stat_smooth(color="red", fill="lightgray", size=1.3) +
+ xlab("Percent Complete")
+ scale_x_continuous(labels = percent) + my.theme)
```

And, finally, render our plot.

```{r}
Plot(ggplot(sentences, aes(pct.complete, readability))
+ ylab("Automated Readability Index")
+ ggtitle("Simplicity of Clement's Argument"))
```

![plot](http://patellis.files.wordpress.com/2014/03/rplot01.png?w=960)

By visualizing Mr. Clement’s argument as a measure of readability, we can see that his rhetoric is, for the most part, relatively simple. According to the Automated Readability Index, an U.S.-educated tenth grader should be able to comprehend the argument (approximately). So either tenth graders are getting a whole lot smarter or I didn’t pay attention enough in high school history classes. Of course, this measure is simply an approximation. Regardless, the overall semantic simplicity of Mr. Clement’s advocacy is apparent. Though this little experiment by no means satisfies the scientific method, this further suggests that simplicity is an important driver of effective oral advocacy.

That said, what works for the LeBron James of SCOTUS might not work for another advocate. In the words of Justice Harlan II:

> The art of advocacy — and it is an art — is a purely personal effort, and as such, any oral argument is an individualistic performance. Each lawyer must proceed according to his own lights, and if he tries to cast himself in the image of another, he is likely to become uneasy, artificial, and unpersuasive.
