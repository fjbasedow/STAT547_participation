all: report.html

clean:
	rm -f words.txt histogram.tsv histogram.png report.html

words.txt:
	Rscript -e 'download.file("https://svnweb.freebsd.org/base/head/share/dict/web2?view=co", destfile = "words.txt", quiet = TRUE)'

histogram.tsv: histogram.R words.txt
	Rscript $< # $< refers to first dependency

histogram.png: histogram.tsv
	Rscript -e 'library(ggplot2); qplot(Length, Freq, data=read.delim("$<")); ggsave("$@")' #seperate different lines of R code with semicolons
	rm -f Rplots.pdf

report.html: report.rmd histogram.tsv histogram.png
	Rscript -e 'rmarkdown::render("$<")'