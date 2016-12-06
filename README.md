# EDirect_EUtils_API_Cookbook


### The following items are reasonable to post on this page:
    Pull requests with working EDirect scripts
    Issues citing specific use cases and requesting EDirect scripts
    Issues presenting non-working EDirect scripts looking for fixes
    Pull requests with modifications or optimization of EDirect scripts on the page.
### Best Practices for EDirect:
    Please keep to <50,000 expected hits (it simply won’t work)
    Please do not run from multiple processors on a compute farm

### All items below come with no explicit or implicit warranty.  All code is as-is and produced for the bioinformatics community, from the bioinformatics community.  

| Operation                                                                          	| Author     	| Draft EDirect Script                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         	| Edited (vetted) EDirect Script (anyone)                                                                                                                                                                                                                                                                                                               	|   	|
|------------------------------------------------------------------------------------	|------------	|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|---	|
| Gene Aliases                                                                       	| NCBI Folks 	| esearch -db gene -query   "Liver cancer AND Homo sapiens" \| efetch -format docsum \| xtract   -pattern DocumentSummary -element Name OtherAliases OtherDesignations                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Genomic sequence fastas   from RefSeq assembly for specified taxonomic designation 	| NCBI Folks 	| wget `esearch -db assembly   -query "Leptospira alstonii" | efetch -format docsum | xtract   -pattern FtpPath -sep "\n" -element FtpPath | grep GCF | awk   -F"/" '{print $0"/"$NF"_genomic.fna.gz"}'`                                                                                                                                                                                                                                                                                                                                                                                                                                                                       	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Get organellar contigs from   genbank                                              	| NCBI Folks 	| esearch -db nuccore -query "LKAM01" \| efetch   -format fasta                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Get   protein sequences from nucleotide accessions                                 	| NCBI Folks 	| cat accs_file \| epost -db   nuccore -format acc \| elink -target protein \| efetch -format fasta                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Complete taxonomy (KPCOFG)   for taxids.                                           	| NCBI Folks 	| efetch -db taxonomy -id 9606,1234,81726 -format xml \|   xtract -pattern Taxon -tab "," -first TaxId ScientificName -group   Taxon -KING "(-)" -PHYL "(-)" -CLSS "(-)" -ORDR   "(-)" -FMLY "(-)" -GNUS "(-)" -block   "*/Taxon" -match "Rank:kingdom" -KING ScientificName   -block "*/Taxon" -match "Rank:phylum" -PHYL   ScientificName -block "*/Taxon" -match "Rank:class" -CLSS   ScientificName -block "*/Taxon" -match "Rank:order" -ORDR   ScientificName -block "*/Taxon" -match "Rank:family"   -FMLY ScientificName -block "*/Taxon" -match "Rank:genus"   -GNUS ScientificName -group Taxon -tab "," -element   "&KING" "&PHYL" "&CLSS"   "&ORDR" "&FMLY" "&GNUS" 	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Obtain UniProt IDs from   gene symbols                                             	| NCBI Folks 	| esearch -db gene -query "tp53[preferred symbol] AND   human[organism]" \| elink -target protein \| esummary \| xtract -pattern   DocumentSummary -element Caption SourceDb \| grep -E '^[OPQ][0-9][A-Z0-9]{3}[0-9]\|^[A-NR-Z][0-9]([A-Z][A-Z0-9]{2}[0-9]){1,2}'                                                                                                                                                                                                                                                                                                                                                                                                                 	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Retrieve   Taxon IDs from list of genome accession numbers                         	| NCBI Folks 	| cat genome_accession.txt \|   epost -db nuccore -format acc \| esummary \| xtract -pattern DocumentSummary   -element AccessionVersion TaxId                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Convert Article's DOI to   PMID                                                    	| NCBI Folks 	| esearch -db pubmed -query   "10.1111/j.1468-3083.2012.04708.x" \| esummary \| xtract -pattern   DocumentSummary -block ArticleId -sep "\t " -tab "\n"   -element IdType,Value \| grep -E '^pubmed\|doi'                                                                                                                                                                                                                                                                                                                                                                                                                                                                          	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Access organism specific   meta-data from NCBI genome database                     	| NCBI Folks 	| esearch -db genome -query "22954[uid]" \| elink   -target bioproject \| efetch -format xml \| xtract -pattern DocumentSummary   -element Salinity OxygenReq OptimumTemperature TemperatureRange Habitat                                                                                                                                                                                                                                                                                                                                                                                                                                                                         	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Get the status of records   from PubMed search                                     	| NCBI Folks 	| esearch -db pubmed -query "pde3a AND 2016[dp]" \|   esummary \| xtract -pattern DocumentSummary -element Id RecordStatus                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Sort the hits by sequence   length in nucleotide database                          	| NCBI Folks 	| esearch -db nuccore -query   "bacillus[orgn] AND biomol_rRNA[prop] AND 1500:1560[slen]" \|   esummary \| xtract -pattern DocumentSummary -element Slen Extra \| sort -rnk 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Getting meta data from   assembly                                                  	| NCBI Folks 	| esearch -db assembly -query "mammals[orgn] AND   latest[filter]" \|efetch -format docsum \|xtract -pattern DocumentSummary   -element Organism,SpeciesName,BioSampleAccn,LastMajorReleaseAccession     -block Stat -if "@category" -equals chromosome_count -element Stat   \|grep -Pv "\t0$"                                                                                                                                                                                                                                                                                                                                                                                   	|                                                                                             	|   	|
| Fetch HSPs from a BLAST hit   in FASTA.                                            	| NCBI Folks 	| blastn -db nr -query in.fna -remote -outfmt "6 sacc   sstart send" \| xargs -n 3 sh -c 'efetch -db nuccore -id "$0"   -seq_start "$1" -seq_stop "$2" -format fasta'                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           	|                                                                                                                                                                                                                                                                                                                                                       	|   	|
| Get all Gene Ontology IDs for a given gene name | NCBI Folks | script here | |  