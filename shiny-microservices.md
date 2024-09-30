# shiny-microservices

R/Shiny application for testing LIS microservices instances.

I wrote this to make testing the microservices easier than remembering the correct `curl` syntax.

### Links

[GitHub repository](https://github.com/legumeinfo/shiny-microservices)

[Running instance](http://dev.lis.ncgr.org:50080/shiny/shiny-microservices/) (on sgr-shiny)

### Notes

List your available microservices URLs in `microservices-urls.txt`. The above instance has a choice of two:

`https://gcv-microservices.lis.ncgr.org/lis`: works for **Genes**, **Chromosome**, **Micro-Synteny Search**, **Macro-Synteny Blocks**, **Search**, **Chromosome Region**.

`https://services.lis.ncgr.org`: works for **Gene Linkouts**, **Genomic Region Linkouts**.

### To do

Add the [macro_synteny_paf](macro-synteny-paf.md) microservice.

Refactor GET and POST actions.
