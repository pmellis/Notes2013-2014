'''{r}
Appellate_Disposition <- c(23689,2753,2249,516,68)
names(Appellate_Disposition) <- c("Affirmed", "Dismissed", "Reversed", "Remanded", "Other")
pareto.chart(Appellate_Disposition, ylab = "Frequency", col=heat.colors(length(Appellate_Disposition)))
'''
