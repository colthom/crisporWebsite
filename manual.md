Input form {#inputform}
==========

Sequence Name: You can give your input sequence an optional name. It will be shown on the output page and appended to all oligonucleotides that you download later. 

Input Sequence: The input sequence should be a genomic DNA sequence, not cDNA. Case will be retained in the output, so you can mark exons, ATGs or reading frame using upper/lowercase. If the input sequence contains N-characters (="aNy" nucleotide), no guides will go over these characters, so you can mark positions that you want to exclude from the design with Ns, e.g. to avoid single nucleotide polymorphisms (SNPs, also sometimes called SNVs, single nucleotide variants).

Genome: Select your genome of interest. Your input sequence should usually be contained in this genome, but it may not be present, e.g. if you are designing guides against a transgene, like GFP. A warning will appear in this case. We have numerous genomes already imported. Some species are available multiple times, e.g mouse and human, because assemblies from different years are available and some loci are only part of certain assembly releases. Also, the annotation with SNPs (see below) is only available for certain assemblies. This is shown as part of the genome list, e.g. the 1000 Genomes variant annotation is only available for the human genome assembly called "hg19" (aka GrCh37).

If your genome is not on the list, please contact us and send us a link to the fasta file and ideally also GFF gene annotations and the common and scientific names, e.g. "zebrafish" and "Danio rario". If your genome is in NCBI RefSeq or on the UCSC or Ensembl browsers, please send us the NCBI accession ID or a link to the UCSC or Ensembl page.

Annotated input sequence {#annotseq}
========================

The main output of the tool is a page that shows the annotated input sequence at the top and the list of possible guides in the input sequence at the bottom. 

The input sequence is shown first. Underneath the sequence, all PAM (Protospacer adjacent motif) sites are highlighted. Most labs use a derivative of Sp-Cas9, where the PAM is NGG. Sp-Cas9 does not cut at an exact position, usually 1bp-3bp 5' of the PAM. These three basepairs 5' of the PAM are marked with dashes ("-"). PAM sites can also be on the reverse strand of the input sequence. For Sp-Cas9, these correspond to CCN motifs on the input sequence and the direction of the dashes is to the right.

PAMs are clickable and link to the table of targets below (see next section).

The input sequence can be annotated with variants, mostly SNPs, found in the genome. The variants are shown above the input sequence. You can hover with your mouse over them, to show details about the variant, usually the nucleotide change (T->G) and the frequency. You can change the variant database and you can also set a minimum frequency for variants shown on the page. Any variant that has a frequency below the threshold will not be displayed. Note that very few genomes have variants in our database at the moment. If you want us to add one, contact us by email and send us the URL of the database or the VCF file.

Guide list {#guidelist}
==========

Shown below the input sequence are the guide target sequences, one per PAM. For spCas9, the targets are 20bp long. Each 20bp target sequence in the input sequence is aligned against the whole genome allowing at most four mismatches and the results are summarized as a table. The table has the columns described below. To sort by one of them, click its name in the first row.

Column 1: the "name" of the target, simply the position of the PAM on the input sequence and the strand. You can sort by position by clicking the first row with the description "Position/Strand" in the table.

Column 2: the target sequence with the variants (if available for this genome) underneath. High GC content targets are flagged, as are low-GC content targets. Various studies have reported that both cases lead to low target efficiency. Also shown in this column are restriction enzymes that overlap the three basepairs 5' of the PAM site. Finally, one of the most important features of CRISPOR is available here, under the link "PCR primers" (see the [Primers section](#primers) below). 

This column allows to filter the guides by the first nucleotide, as some RNA expression promoters give much more efficient transcription when starting with specific nucleotides. Some labs only use targets that start with G- for the U6 promoter, A- for the U3 promoter or G- for the T7 promoter. In this way, no further sequence changes are necessary. In our lab, we do not select guides, but simply prefix them with the required nucleotide (see below).

Column 3: the specificity score is a prediction of how much an RNA guide sequence for this target may lead to off-target cleavage somewhere else in the genome. The score ranges from 0-100 with 100 being the best, meaning the search could not find a single sequence in the genome that differs from the target at up to four positions. This score uses the formula from the MIT Crispr Website but with a better and more sensitive search engine. We think that good guides should have a specificity score of at least 50, based on the data from whole-genome off-target assays, see Figure 3a in the [CRISPOR paper](http://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-1012-2). The coloring red/yellow/green is based on the specifcity score (>50 = green, >30 = yellow). You should really avoid guides with very low scores, unless you can validate the off-targets with special assays or you can cross the animals until you are sure that no off-targets are left. 

Column 4: the efficiency score (0-100) is a prediction of how much this target may be cut by its RNA guide sequence. You have to choose one of two scores here: the scoring method from the Doench 2016 paper (aka "sgRNA Designer") or the one by Moreno-Mateos 2016 ("CrisprScan"). The Doench2016 score is the best score for guides expressed in the cells, with a U6 promoter. The Moreno-Mateos2016 score is better when the guide is expressed in-vitro with a T7 promoter. See Figure 4 and 5 in the [CRISPOR paper](http://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-1012-2). Also, the predictive power of this score is not great, with a correlation of around 0.4 with the assay. If you have no choice and need to pick certain guides, you want to ignore this score. If you have a choice however, or you are screening many guides, the efficiency score should lead to a higher percentage of highly efficient guides.

Column 5: the out-of-frame score (0-100) is a prediction how much a score leads to out-of-frame deletions. This is relevant if you are doing gene-knockouts with a single guide. Gene-knockouts with single guides work because repair after DNA cutting is error-prone and small deletions are introduced 5' of the PAM. It has been observed by Bae et al. that this repair does not lead to a random distribution of small deletions, but that certain deletions are favored, depending on their flanking DNA sequences. You can click on this score to show what the predicted deletions are. We know of at least one case where it was impossible to obtain a gene knock-out with a specific guide because all deletions were in-frame (T. Momose, unpublished data). The out-of-frame score is higher the more deletions have a length that is not a multiple of three, see [Bae et al.](http://www.nature.com/nmeth/journal/v11/n7/full/nmeth.3015.html).

Column 6: the number of possible off-targets in the genome, for each number of mismatches. This is summarizing the result from the whole-genome search for sequences similar to the target sequence. This is best explained by an example: a description "0 - 1 - 2 - 9 - 28" means that the target matches 0 locations in the genome with no mismatch, 1 location in the genome with 1 mismatch, 2 locations with 2 mismatches, 9 with 3 and 28 locations with 4 mismatches. The smaller numbers in grey below use the same scheme, but only for locations with no mismatch in the 12 bp close to the PAM, the "seed" region. There have been reports that off-targets with a single mismatch in the seed region are never cut. If the grey numbers are "0 - 0 - 1 - 7 - 2" this means that there is one location in the genome where the target matches with three mismatches and all three mismatches are outside the seed region, 7 locations with 4 mismatches outside the seed region and 2 locations with four mismatches outside the seed region. The total number of off-targets is shown in this column, too.

Why only search allowing only up to four mismatches? When we looked at real off-targets, around 90% of them had not more than four mismatches. While you can increase the number of allowed mismatches using the command line version of CRISPOR to five or even six mismatches, the large majority of the predicted locations will be false positives, and most guides would require thousands of PCRs to check all predicted off-targets. See Figure 1 of the [CRISPOR paper](http://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-1012-2)

Column 7: here we list the locations of all possible off-targets, whether they fall into an exon, intron or between genes and the closest gene. You can hover with your mouse over these to show where the mismatches are. By default only the three most likely off-targets are shown, click on "show all" to see more.

All off-targets are sorted by an "off-target score" which tries to predict what the most likely off-targets are. The score is based on where the mismatches are (close to PAM = less likely) and what the exact nucleotide change is. We have benchmarked four different scores (Figure 2 of the [CRISPOR paper](http://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-1012-2)) and found the CFD score slightly more effective than the MIT score, compared to the two others. The mouse-over shows both scores.

For some genomes, there is no gene annotation. This is the case when we could not find a valid gene model (GFF) file for the genome. Do not hesitate to contact us by email if you have a gene model file or want us to add one.

Primers
=======

FAQ
===

* Why is the MIT score (aka Hsu score) displayed by CRISPOR different from the ones displayed by the MIT website, Desktop Genetics or Benchling?

Because these other tools do not find as many off-targets as CRISPOR. As we
have shown in our paper, CRISPOR finds all off-targets at 4 mismatches. The MIT
websites does not find all off-targets, because it filters for repeats.
Benchling misses some off-targets, not as many as the MIT website but still a
high number. We do not know why, but it may also be related to repeats. Desktop
Genetics also searches for up to three mismatches.

* Can I score existing guides with CRISPOR ?

Sure, just enter the target sequence, so the guide + the PAM sequence, into the box.

* Can you add my genome?

Yes, please send us the UCSC genome name or the Ensembl name or the NCBI
Assembly ID, which starts with GCF_ or GCA_. We can also add individual FASTA
and GFF files, which you can send by URL or as a Dropbox link.

* Can you add my genome and not share it?

Yes, we can add private genomes. People can use them on the website, but they
will not be able to download the genome. Just tell us that the genome is
private and shall not be downloadable.

* Can I search for 19bp long guides?

Yes, but only with the command line version. It has the --shortGuides option.
Look for the command line version on our "Download" pages. If you have trouble,
email us. This is a very rarely used option which is why we have not exposed 
it on the web interface.




