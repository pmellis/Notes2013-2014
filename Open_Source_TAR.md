##Open Source Technology Assisted Review . . . FOR FREE!

Consumers of e-discovery and document management services and products are at an inherent disadvantage. Costs are unpredictable, information is asymmetric, and technology is constantly changing. At the same time, electronic data, both structured and unstructured, is growing at an exponential rate and so is the need for faster, more efficient, and intelligent information management and e-discovery solutions. Whether you are General Counsel with a large organization or a solo practitioner, an understanding of the identification, production, storage, and deletion of electronic information and the ability to analyze that information is crucial for assessing potential risk or litigating a case moving forward, i.e. being a lawyer.

But some lawyers are at a greater disadvantage than others.

For instance, a few weeks ago, a criminal defense attorney spoke at MSU Law and discussed how e-discovery influences his work. To me, his message and its implications for his clients were bleak. E-discovery not only impacts his work, but more importantly, can mean the difference between a “guilty” and “not guilty” verdict for his clients. In cases where electronic information is an issue, this defense attorney does his best to work with his resources, the client, and the court to obtain any relevant evidence that may help his client’s case. Yet, access to a lot of potential electronic evidence, a deleted text message for example, requires the expensive work of a computer forensics expert or some other burdensome method of retrieval.

Even more common and perhaps more taxing to this attorney, a solo practitioner, is receiving hundreds if not thousands of documents related to a case and manually reviewing them. Even something as simple as locating duplicate documents can take hours, if not days. The majority of clients this attorney represents cannot afford technology assisted review (“TAR”), leaving their case to the mercy of human error and time limitations. (For more on e-discovery and criminal law, check out E-Discovery in Criminal Cases: A Need for Specific Rules, by Daniel B. Garrie & Daniel K. Gelb).

I assume that some of the problems encountered by this criminal defense attorney are also common for small firm and solo-practitioners on the civil side, often buried in electronic information without the resources to contract a vendor or implement a TAR platform. In sum, the proliferation of electronic information is creating a divide for attorneys: those who have the tools and resources to handle and effectively analyze it, and those who do not.

This past year, I’ve had the opportunity to experiment with a few different document review platforms, including Concordance, Clustify, and Backstop through my e-discovery class at MSU. Although I do not have much to compare these programs to, I thought they all worked well. Clustify and Backstop were much more intuitive and powerful, but nonetheless, each allowed me to perform certain tasks that would otherwise have been time-consuming and expensive within minutes: de-duping, clustering, searches, predictive coding, etc. That said, I do not know the exact costs of these platforms (one of the joys/detriments of being a student using legal technology), but I would guess that each is prohibitively expensive for the average solo or small firm attorney.

So is there a way that lawyers without the resources to use TAR software or hire vendors can harness the power of technology to improve their efficiency and results for clients? In the long-run, the price of e-discovery/document management solutions will likely drop and products will improve and become more user-friendly. But, in the meantime, is there a solution?

I believe there is and it is in the form of open source software. Open source software is software whose source code is available for modification or enhancement by anyone. It is usually free and worked on by large communities of developers working collaboratively to constantly improve the product and offer support to the software’s users. There are a lot of examples of open source software out there (Firefox, Linux, OpenOffice, etc.), but the one that I think has a lot of potential for use in e-discovery is the R Project for Statistical Computing.

According to RevolutionAnalytics.com:

> R is the world’s most powerful programming language for statistical computing, machine learning and graphics as well as a thriving global community of users, developers and contributors. R includes virtually every data manipulation, statistical model, and chart that the modern data scientist could ever need. As a thriving open-source project, R is supported by a community of more than 2 million users and thousands of developers worldwide. Whether you’re using R to optimize portfolios, analyze genomic sequences, or to predict component failure times, experts in every domain have made resources, applications and code available for free, online.

And it is applicable to e-discovery. How? E-discovery platforms and providers are essentially doing the same thing R is doing: Statistics. Whether locating ESI, using predictive-coding to search for relevant documents, or doing something as simple as de-duping a corpus, statistics is the driving-force behind e-discovery solutions . . . unless your solution is having your firm’s associate(s) read through documents or outsourcing the work, offshore or otherwise.

So let’s look at a very simple example of how R might be used to analyze and manage documents:

Document clustering is a popular method of TAR, especially when dealing with a large corpus. In particular, clustering allows for more efficient review by quickly revealing relationships and connections within a set that may not otherwise be apparent and by identifying duplicate documents within a set. Within the broader context of data science, clustering is the task of grouping a set of objects in such a way that objects in the same group (called a cluster) are more similar (in some sense or another) to each other than to those in other groups (clusters).

![PIC](http://upload.wikimedia.org/wikipedia/commons/thumb/0/09/ClusterAnalysis_Mouse.svg/450px-ClusterAnalysis_Mouse.svg.png)

R does this (shown above) and it can be applied to documents. Let’s take a look at a simple example using R (downloadable here) and RStudio (an IDE interface to make R more user-friendly – downloadable here). I am going to just get straight into the code, but if you are new to R, check out Professor Katz’s “R Boot Camp Slides” – that’s how I learned!

First, I’m going to create a simple test corpus consisting of nine short email excerpts from the original Enron corpus. The samples will pertain to three semantically diverse topics:

###Topic 1

1. “To Mr. Ken Lay, I’m writing to urge you to donate the millions of dollars you made from selling Enron stock before the company declared bankruptcy.”

2. “while you netted well over a $100 million, many of Enron’s employees were financially devastated when the company declared bankruptcy and their retirement plans were wiped out”

3. “you sold $101 million worth of Enron stock while aggressively urging the company’s employees to keep buying it”

###Topic 2

4. “This is a reminder of Enron’s Email retention policy. The Email retention policy provides as follows . . . “

5. ”Furthermore, it is against policy to store Email outside of your Outlook Mailbox and/or your Public Folders. Please do not copy Email onto floppy disks, zip disks, CDs or the network.”

6. ”Based on our receipt of various subpoenas, we will be preserving your past and future email. Please be prudent in the circulation of email relating to your work and activities.”

###Topic 3

7. “We have recognized over $550 million of fair value gains on stocks via our swaps with Raptor.”

8. “The Raptor accounting treatment looks questionable. a. Enron booked a $500 million gain from equity derivatives from a related party.”

9. “In the third quarter we have a $250 million problem with Raptor 3 if we don’t “enhance” the capital structure of Raptor 3 to commit more ENE shares.”

Now that we have our documents, we can create our corpus in R Studio. We will do so by first placing all of our sample documents into a vector, “text”, and then cleaning the text (remove stopwords, stemming, etc.) within the corpus. This is accomplished with the following code:

```{r}
# Load requisite packages
library(tm)
library(ggplot2)
library(lsa)
library(SnowballC)

# Place Enron email snippets into a single vector.
text <- c(
    "To Mr. Ken Lay, I’m writing to urge you to donate the millions of dollars you made from selling Enron stock before the company declared bankruptcy.", 
    "while you netted well over a $100 million, many of Enron's employees were financially devastated when the company declared bankruptcy and their retirement plans were wiped out", 
    "you sold $101 million worth of Enron stock while aggressively urging the company’s employees to keep buying it", 
    "This is a reminder of Enron’s Email retention policy. The Email retention policy provides as follows . . .", 
    "Furthermore, it is against policy to store Email outside of your Outlook Mailbox and/or your Public Folders. Please do not copy Email onto floppy disks, zip disks, CDs or the network.", 
    "Based on our receipt of various subpoenas, we will be preserving your past and future email. Please be prudent in the circulation of email relating to your work and activities.", 
    "We have recognized over $550 million of fair value gains on stocks via our swaps with Raptor.", 
    "The Raptor accounting treatment looks questionable. a. Enron booked a $500 million gain from equity derivatives from a related party.", 
    "In the third quarter we have a $250 million problem with Raptor 3 if we don’t “enhance” the capital structure of Raptor 3 to commit more ENE shares.")
view <- factor(rep(c("view 1", "view 2", "view 3"), each = 3))
df <- data.frame(text, view, stringsAsFactors = FALSE)

# Prepare mini-Enron corpus
corpus <- Corpus(VectorSource(df$text))
corpus <- tm_map(corpus, tolower)
corpus <- tm_map(corpus, removePunctuation)
corpus <- tm_map(corpus, function(x) removeWords(x, stopwords("english")))
corpus <- tm_map(corpus, stemDocument, language = "english")
corpus  # check corpus
```

```
# Mini-Enron corpus with 9 text documents
```

Now that we have our mini-Enron corpus, we can create we can a “term-document matrix” that contains and measures the occurrence of certain terms within each document. Then we can use a statistical technique known as multidimensional scaling, a means of visualizing the level of similarity of individual documents within a corpus, to view our “clusters”. We will create our graph using ggplot2, a data visualization package for R:

```{r}
# Compute a term-document matrix that contains occurrance of terms in each email
# Compute distance between pairs of documents and scale the multidimentional semantic space (MDS) onto two dimensions
td.mat <- as.matrix(TermDocumentMatrix(corpus))
dist.mat <- dist(t(as.matrix(td.mat)))
dist.mat  # check distance matrix

# Compute distance between pairs of documents and scale the multidimentional semantic space onto two dimensions
fit <- cmdscale(dist.mat, eig = TRUE, k = 2)
points <- data.frame(x = fit$points[, 1], y = fit$points[, 2])
ggplot(points, aes(x = x, y = y)) + geom_point(data = points, aes(x = x, y = y, 
    color = df$view)) + geom_text(data = points, aes(x = x, y = y - 0.2, label = row.names(df)))
```

![PLOT](http://patellis.files.wordpress.com/2013/12/untitled1.png?w=960)
    
As you can see, the “documents” are positioned on the graph according to semantic similarity. In other words, they are “clustered”. Topic 1 documents are tightly grouped, while Topic 2 and 3 documents are more loosely grouped or clustered. It’s not perfect, but it’s a start.

Another R package, called LSA, might improve our accuracy. LSA, which stands for latent semantic analysis, is an algorithm applied to approximate the meaning of texts, thereby exposing semantic structure to computation. LSA is commonly used by e-discovery platforms and an effective way of grouping documents. LSA should produce superior results because we can “weight” the text and compute distance more effectively. Again, we can display these results graphically.
    
```{r}    
# MDS with LSA
td.mat.lsa <- lw_bintf(td.mat) * gw_idf(td.mat)  # weighting
lsaSpace <- lsa(td.mat.lsa)  # create LSA space
dist.mat.lsa <- dist(t(as.textmatrix(lsaSpace)))  # compute distance matrix
dist.mat.lsa  # check distance mantrix

# MDS
fit <- cmdscale(dist.mat.lsa, eig = TRUE, k = 2)
points <- data.frame(x = fit$points[, 1], y = fit$points[, 2])
ggplot(points, aes(x = x, y = y)) + geom_point(data = points, aes(x = x, y = y, 
    color = df$view)) + geom_text(data = points, aes(x = x, y = y - 0.2, label = row.names(df)))
```   

![PLOT](http://patellis.files.wordpress.com/2013/12/untitled2.png?w=960)

This outcome is dramatically different from the first analysis. As the graph shows, the documents in Topic 3 were pulled down the y-axis quite substantially, while Topic 2 expanded in distance. Topic 3 is not really clustered, although it is obviously distinct from the other Topics. I suspect that my mini-Enron corpus did not have the kind of variety that is really required for the LSA to truly show its computing power and produce robust clusters.

If the results of this mini-Enron test might not persuade you that R is an viable alternative to e-discovery software or vendors, that is my fault. I have only been using R for a couple months now and am still learning how to effectively use it. But, if my example is not persuasive, there is a huge community of developers out there working with R to develop text-mining and document analysis solutions that can sell it better than I can. For example:

- [Here](http://www.r-bloggers.com/text-mining-the-complete-works-of-william-shakespeare/) is an example of a person using R to mine the complete works of William Shakespeare;
- [Here](http://bostondecision.com/2012/05/14/the-text-classifier-determine-the-author-of-a-document-with-machine-learning-in-r/) is an example using R to predict the author of a document based on machine learning from previous documents; and
- [Here](http://www.r-bloggers.com/clustering-the-words-of-william-shakespeare/) is a much more persuasive example of clustering using R with, again, the complete works of William Shakespeare.

These are just a few of the hundreds if not thousands of functions that can be performed on a document set in R and, because it’s open source, there are constantly new techniques in development.

At the end of the day, R comes with a serious learning curve and it simply is not as effective (at least in my hands) as a traditional TAR platform. But, as it continues to develop and lawyers and programmers alike realize its potential in the e-discovery and document management space, I bet that it will become an effective tool for lawyers, even outside of the context of information management. Oh, and one more thing:

**It’s free.**

So for attorneys out there with limited financial resources, R may be an excellent tool for overcoming the divide that electronic information is creating. Therefore, I look forward to continue learning R and its potential to make legal processes more affordable, more efficient, and more intelligent.

