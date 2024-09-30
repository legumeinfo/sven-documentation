# PAF microservice (macro_synteny_paf)

Calls other LIS microservices to generate a PAF file for input to JBrowse (and other applications, in theory).

### Links

[GitHub repository](https://github.com/legumeinfo/microservices/tree/macro-synteny-paf/macro_synteny_paf)
(part of the [LIS microservices](https://github.com/legumeinfo/microservices/tree/macro-synteny-paf))

See the `README` for instructions (about valid URL fields, etc).

### To do

Update after Alan changes macro_synteny_blocks to accept chromosome names.

Once integrated with the other microservices, commit the microservices submodule changes to the [macro-synteny-paf](https://github.com/legumeinfo/gcv-docker-compose/tree/macro-synteny-paf) branch of [gcv-docker-compose](gcv-docker-compose.md).
(Optional - you may have a better way to do it, like independently of GCV)

Cache results so it does not have to recompute them every time the user refreshes the page.
