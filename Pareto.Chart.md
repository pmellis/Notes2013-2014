[Data](http://www.uscourts.gov/uscourts/Statistics/StatisticalTablesForTheFederalJudiciary/2013/june/B05Jun13.pdf)

```{r}
Appellate_Disposition <- c(23689,2753,2249,516,68)
names(Appellate_Disposition) <- c("Affirmed", "Dismissed", "Reversed", "Remanded", "Other")
pareto.chart(Appellate_Disposition, ylab = "Frequency", col=heat.colors(length(Appellate_Disposition)))
```
![pic](http://patellis.files.wordpress.com/2014/04/rplot.png)
