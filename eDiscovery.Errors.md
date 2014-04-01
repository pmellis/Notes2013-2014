A Fatal eDiscovery Error
=========================

![pic](http://patellis.files.wordpress.com/2014/02/high-risk-pic.jpg?w=563&h=316)

eDiscovery and doc review vendors and software solutions dominated the [LegalTech Trade Show](http://www.legaltechshow.com/r5/cob_page.asp?category_code=ltech) this past week in New York City. While there were a couple of standouts in my mind, most notably [Logikull](http://logikcull.com/), the majority of solutions boasted similar claims rooted in technical concepts. Of course, eDiscovery and document review in the [“Age of Big Data”](http://www.catalystsecure.com/pdfs/Catalyst_Article_IG_and_eDiscovery_in_the_Age_of_Big_Data.pdf) is no simple task and requires a level of technicality that I’m sure is difficult to sum up in a simple pamphlet or trade show display. That said, understanding what’s going on under the hood of vendor tech or software is crucial when making an intelligent choice for your firm or company’s needs.

Today in Legal Analytics, taught by Professor Katz and Professor Bommarito, we discussed some of the metrics that should be considered when selecting an eDiscovery or document review solution powered by machine learning, including precision, recall, accuracy, and the trade-offs in price that alterations in these metrics can yield.

One concept that I thought was particularly interesting (and worth sharing) is the relationship between errors in responsive and non-responsive documents and the problems that these errors can cause for vendors, firms, and most importantly, the client. In the context of ML-based classification, the most commonly used task for eDiscovery/doc reviewers, we can determine the accuracy of a classifier using external judgments, frequently described as true positives, true negatives, false positives, and false negatives.  The terms positive and negative refer to the classifier’s prediction (the expectation), and the terms true and false refer to whether that prediction corresponds to the external determination. This relationship can be visualized with the following chart:

![plot](http://patellis.files.wordpress.com/2014/02/3eglc.png?w=563&h=257)

Putting these concepts in terms that are more familiar in the eDiscovery context:

- a true positive is a relevant document that is produced as relevant;
- a true negative is a irrelevant document that is produced as irrelevant;
- a false positive is a irrelevant document that is produced as relevant; and
- a false negative is a relevant document that is produced as irrelevant.

This is where the commonly referenced eDiscovery metrics “recall” and “precision” come into play. Recall is the true positive rate or “sensitivity”, and precision is also referred to as positive predictive value. Finally, the true negative rate is also called “specificity”. These more granular metrics are likely better measures of a system’s quality. Because while a vendor may boast a 95% measure of “accuracy“, the system may still create catastrophic errors, of the Type I and II variety.

So, which of these errors is more devastating in the context of litigation? In my opinion, a Type II error is potentially much worse – even fatal. Why? With a Type I error, documents that are irrelevant will be tagged as relevant. This might cause some extra time, work, and money after the TAR or vendor has done its work. Perhaps it could even cause some embarrassment or cost-shifting in court if opposing counsel can show that you produced lots of irrelevant documents. But consider the alternative.

A Type II error could cause a relevant document to be classified as irrelevant. In other words, the one in a million smoking gun email could be cast into the abyss. But that’s what quality control is for, right? Not exactly. Thus, when evaluating a eDiscovery/doc review platform, understanding how the system combats the Type II error is essential. Along with that should come the understanding that no solution is perfect – human or machine – yet.

