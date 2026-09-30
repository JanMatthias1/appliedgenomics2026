## Assignment 3: Variant Calling
Assignment Date: Wednesday, September 30, 2026 <br>
Due Date: Wednesday, October 7, 2026 @ 11:59pm <br>

### Assignment Overview

In this assignment you will implement the requirements and applications for variant calling. We recommend python for all of the programming exercises. See the resources at the bottom of the page for tips for the variant calling exercises.

As a reminder, any questions about the assignment should be posted to [Piazza](https://piazza.com/class/mt9gq0k8adb7l7#).


### Question 1. BWT Encoding [20 pts]

1a. In the language of your choice, implement a BWT encoder and encode the string below. Faster (Linear time) methods exist for computing the BWT, although for this assignment you can use the simple method based on standard sorting techniques. Your solution does *not* need to be an optimal algorithm and can use O(n^2) space and O(n^2 lg n) time. 

Here is the recommended pseudo code (make sure to submit your code as well as the encoded string):

```
computeBWT(string s)
  ## add the magic end-of-string character
  s = s + "$"
 
  ## build up the BWM from the cyclic permutations
  ## note the ith cyclic permutation is just "s[i..n] + s[0..i]"
  rows = []
  for (i = 0; i < length(s); i++)
    rows.append(cyclic_permutation(s, i))

  ## just use the builtin sort command to sort the cyclic permutations
  sort(rows)

  ## now extract the last column
  bwt = ""
  for (i = 0; i < length(s); i++)
    bwt += substr(rows[i], length(s), 1)
  return bwt
```

String to encode:
```
I_am_fully_convinced_that_species_are_not_immutable;_but_that_those_belonging_to_what_are_called_the_same_genera_are_lineal_descendants_of_some_other_and_generally_extinct_species,_in_the_same_manner_as_the_acknowledged_varieties_of_any_one_species_are_the_descendants_of_that_species._Furthermore,_I_am_convinced_that_natural_selection_has_been_the_most_important,_but_not_the_exclusive,_means_of_modification.
```

1b. The original motivation for the BWT was for data compression, especially through a runlength encoding where repeats ("runs") of a character are replaced by a count. For example the string "GGGGAAAAATTTTTTACCCCCCCCAAAAAAA" (30 characters) can be encoded as "G4A5T6AC8A7" (11 characters) (36% of the original length). Implement a runlength encoding algorithm and compute 1) the length of the original string from 1a, 2) the length of a runlength encoded version of the same string, and 3) the length of the runlength encoded version of the BWT of that same string

1c. Now compute the effectiveness of a runlength encoding and the BWT on a 10kb segment of human chromosome 22. Make sure to report 1) the length of the original string, 2) the length of the runlength encoded version of the same string, and 3) the length of the runlength encoded version of the BWT of that string. Before running rle or bwt, clean the fasta file by replacing lowercase acgt with ACGT, replacing any other characters with N, removing the fasta header, and removing all the newlines (the genome sequence should appear by itself on one very long line of text)

```
    $ wget https://github.com/schatzlab/appliedgenomics2026/raw/refs/heads/main/assignments/assignment1/chr22.fa.gz
    $ gunzip chr22.fa.gz
    $ samtools faidx chr22.fa
    $ samtools faidx chr22.fa chr22:20000000-20010000 > chr22_orig.fa
    
    $ clean_fasta.py chr22_orig.fa > chr22.clean
    $ compute_length.py chr22.clean
    
    $ rle.py chr22.clean > chr22.clean.rle
    $ compute_length.py chr22.clean.rle
    
    $ bwt.py chr22.clean > chr22.bwt
    $ rle.py chr22.bwt > chr22.bwt.rle
    $ compute_length.py chr22.bwt.rle  
```


### Question 2. Binomial Distribution [20 pts]

- 2a. For coverage n = 10 to 200, calculate the maximum number of minor allele reads (round down) that would make your one-sided binomial test reject the null hypothesis p=0.5 at 0.05 significance. Plot coverage on the x-axis and (number of reads)/(coverage) on the y-axis. Note this is the minimum number of reads that are necessary to believe we might have a heterozygous variant on the second haplotype rather than just mere sequencing error.

- 2b. What asymptote does the plot seem to approach? Why is this?



### Question 3. Identifying a Specific Variant [20 pts]

*Three friends, Ezra, Sabine, and Zeb go out to lunch together and, in discussing the menu, all realize that they all have a specific culinary preference. They believe the cause of this preference to be genetic. Can you identify what SNP causes this preference and what the preference is?*

Download the read set from here: [https://github.com/schatzlab/appliedgenomics2026/tree/main/assignments/assignment3/input_files](https://github.com/schatzlab/appliedgenomics2026/tree/main/assignments/assignment3/input_files)

For this question, you may find this tutorial helpful: [https://hbctraining.github.io/In-depth-NGS-Data-Analysis-Course/sessionVI/lessons/02_variant-calling.html](https://hbctraining.github.io/In-depth-NGS-Data-Analysis-Course/sessionVI/lessons/02_variant-calling.html)

To answer the following questions, you will need to run several alignment and variant calling tools. You can run these on the command line or embed them into a jupyter notebook, but make sure to record the exact commands you used.

- 3a. You are able to narrow down the SNP to a specific region in chromosome 11. Using bowtie2, align their FASTQ files to the provided reference file. How many reads does each file have and how many are successfully mapped? [Hint: Use `samtools flagstat`.] 

- 3b. How many high confidence SNPs and indels does each file have? [Hint: Sort the SAM file first. Then, call variants with `freebayes`. Then filter for the variant quality to be at least 30 and normalize using `bcftools`. Summarize using `bcftools stats`.]

- 3c. You now know which SNPs and indels each friend has. However, you want to know which variant they all share. How many variants are shared between all three friends? [Hint: Use `bcftools isec` after filtering and normalizing the variants.]

- 3d. Between the variants shared between all three friends, which is likeliest to cause a phenotype of interest? [Hint: The variant should be homozygous in all 3 samples and will be in a gene that has a function related to taste. You can search for variants at a certain chromosome and position at https://genome.ucsc.edu/cgi-bin/hgTracks?db=hg38. Remember, the position in the intersected VCF is the position within the region we're looking at, so you will have to find the starting location of the region by shifting over by 5868417! Use `bcftools view -i 'GT="1/1"' PREFIX.vcf` to filter for the correct genotype.]

- 3e. What is the phenotype? [Hint: Search the name of the gene associated with the variant you found in 3d.]


### Question 4. Planetary Population Analysis [20 pts]

You have synthetic genotypes from five populations: **mercury, venus, mars, saturn, and pluto**. Explore their genetic relationships using PCA, t-SNE, and UMAP, then train a classifier to predict the population of origin of additional unlabeled people. Infer which populations are most similar from your analysis.

Download the files from the [planets directory](planets/):

| File | People | Columns |
| --- | ---: | --- |
| [planets/labeled_genotypes.csv](planets/labeled_genotypes.csv) | 500 (100 per population) | `sample_id`, `population`, `variant_001` through `variant_100` |
| [planets/challenge_genotypes.csv](planets/challenge_genotypes.csv) | 50 | `sample_id`, `variant_001` through `variant_100` |

Each person has 100 binary variant values (`0` or `1`). These are simplified allele indicators, rather than diploid `0/1/2` allele dosages. Rows are shuffled, and the challenge population labels are hidden. Sample IDs identify people and should not be used as genetic features.

Use Python with pandas, NumPy, matplotlib, scikit-learn, and umap-learn (see the resources below). A Jupyter notebook is recommended. Each part should require only about 20 lines of code; you can reuse a small plotting helper. Use `random_state=2026` for stochastic methods and the validation split, and keep population colors consistent across plots.

- 4a. Load the files with `pandas.read_csv`. Build a feature matrix `X` containing only the 100 variant columns and a label vector `y` from `population`. Use the same variant column order for the challenge matrix. Report the two feature matrix shapes, confirm that the genotypes contain only zeros and ones, and count the labeled people in each population. Explain why `sample_id` and `population` should be excluded from `X`.

- 4b. Use `sklearn.decomposition.PCA(n_components=2)` to visualize the labeled genotypes. Plot one point per person, colored by population, with a legend. Label the axes with the percentage of variance explained by each component using `explained_variance_ratio_`. Use the original binary values; PCA automatically centers the columns. What fraction of the variance do the two components explain together? Which populations appear most similar, and which are clearly separated?

- 4c. Use `sklearn.manifold.TSNE` with `n_components=2`, `perplexity=30`, `init="pca"`, `learning_rate="auto"`, and `random_state=2026`. Apply it to the original 100 variant columns and plot the result as above. Which populations appear most similar in this plot? Does this agree with PCA? Why should you be cautious about interpreting the distances between separate t-SNE clusters as genetic distances?

- 4d. Use `umap.UMAP` with `n_components=2`, `n_neighbors=15`, `min_dist=0.1`, `metric="hamming"`, `random_state=2026`, and `n_jobs=1`. Hamming distance is the fraction of variants at which two genotypes differ. Call `fit_transform(X)` without passing population labels, then use the labels to color the plot. Which populations appear most similar in UMAP? Compare your conclusions across all three plots, including any disagreement or uncertainty. Which method most clearly separates the populations? The first UMAP run may take longer while its numerical code compiles.

- 4e. Split the labeled data into 80% training and 20% validation using `sklearn.model_selection.train_test_split` with `stratify=y` and `random_state=2026`. Train a `sklearn.neighbors.KNeighborsClassifier` with `n_neighbors=5`, `metric="hamming"`, and `algorithm="brute"` on the **100 original variant columns**. Report validation accuracy and plot a confusion matrix using `sklearn.metrics.ConfusionMatrixDisplay`. Fit the classifier only on the training split during validation. What accuracy would random guessing achieve? Based on your plots, which populations would you expect to be confused, and does the confusion matrix support that expectation? Does overlap in a 2D plot necessarily imply poor classification?

- 4f. After validation, refit the classifier on all 500 labeled people and predict the populations of the 50 challenge people. Save `predictions.csv` with exactly two columns, `sample_id` and `predicted_population`, in the original challenge file order and print into your solutions PDF. Keep each prediction attached to the correct sample ID. Make predictions from the genotypes without imposing a particular number of predictions per population. 

### Packaging

The solutions to the above questions should be submitted as a single PDF document that includes your name, email address, and all relevant figures (as needed). If you use ChatGPT for any of the code, also record the prompts used. Submit your solutions by uploading the PDF to [GradeScope](https://www.gradescope.com/courses/1370921), and remember to select where in your submission each question/subquestion is. The Entry Code is: 7B4VJ6. 

For Question 4, include your code and the complete prediction table from `predictions.csv` in the PDF, along with your figures and discussion answers.

If you submit after this time, you will use your late days. Remember, you are only allowed 4 late days for the entire semester!



### Resources


#### [Bowtie2](https://github.com/BenLangmead/bowtie2) - Short read aligner

```
## Install bowtie2 and samtools via mamba/conda
$ mamba create -n assignment3 bowtie2 samtools freebayes bcftools
$ mamba activate assignment3

## Build a bowtie2 index (BWT)
$ bowtie2-build ref.fa ref

## Now align reads, sort, and compute alignment stats
$ bowtie2 -x ref -1 PREFIX.1.fq -2 PREFIX.2.fq -S PREFIX.sam
$ samtools sort PREFIX.sam -o PREFIX.bam
$ samtools flagstat PREFIX.bam > PREFIX.flagstat
```


#### [FreeBayes](https://github.com/ekg/freebayes) - Small variant identification

```
## run freebayes of the sorted alignments against the reference
$ freebayes -f ref.fa PREFIX.bam > PREFIX.vcf
```

#### [bcftools](https://samtools.github.io/bcftools/bcftools.html) - VCF summary

```
## filter, normalize, and compute stats
$ bcftools view -i 'QUAL>=30' PREFIX.vcf -Oz -o PREFIX.filtered.vcf
$ bcftools norm -f ref.fa PREFIX.filtered.vcf > PREFIX.norm.vcf
$ bcftools stats PREFIX.norm.vcf > PREFIX.norm.vcf.stats

## Compress and index the variants
$ bgzip PREFIX.norm.vcf
$ bcftools index PREFIX.norm.vcf.gz

## Now compare the variants in the three samples
$ bcftools isec PREFIX1.norm.vcf.gz PREFIX2.norm.vcf.gz PREFIX3.norm.vcf.gz -p norm -n=3
```


#### Python libraries for population analysis

```bash
python -m pip install numpy pandas matplotlib scikit-learn umap-learn jupyterlab
jupyter lab
```

The package `umap-learn` is imported as `umap`; scikit-learn is imported as `sklearn`. Run your notebook from the assignment directory so paths such as `planets/labeled_genotypes.csv` work.

- [pandas.read_csv](https://pandas.pydata.org/docs/reference/api/pandas.read_csv.html): load the genotype tables.
- [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) and [t-SNE](https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html): dimensionality reduction with scikit-learn.
- [UMAP](https://umap-learn.readthedocs.io/en/latest/api.html): dimensionality reduction with umap-learn.
- [train_test_split](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html), [KNeighborsClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.KNeighborsClassifier.html), and [ConfusionMatrixDisplay](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.ConfusionMatrixDisplay.html): validate and evaluate your classifier.
