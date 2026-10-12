# Project Ideas

Here are selected projects organized by theme. You are also welcome to propose your own idea. A good project has:

1. A well-defined biological or computational question.
2. A concrete method and a baseline for comparison.
3. Appropriate data and computing resources available at the start.
4. A measurable outcome that can be completed before the December presentations.

A successful project can apply a technique from another context, reproduce and extend an important result, or improve an existing method's accuracy, speed, memory use, or generalization. A careful benchmark that explains when and why methods succeed or fail is also a strong project. Developing an entire assembler, basecaller, or foundation model from scratch is usually too broad for the time available.

Each idea below includes starting resources, an initial framing, and an optional extension. The initial framing provides a focused starting point for the project. Graduate students are expected to work toward the optional extensions or propose a comparable extension of their own. Begin with a small dataset, selected genomic regions, or existing predictions before scaling up. For ML projects, design train/test splits that address biological generalization (for example, holding out donors, studies, chromosomes, or related sequences), and check for overlap with pretrained models' training data when possible. Confirm data access, software compatibility, and compute requirements while preparing your [proposal](proposal.md).

## Basecalling and Signal Processing

### 1. Where do current nanopore basecallers make errors?

- **Question:** How do basecalling accuracy and runtime vary across sequence contexts, organisms, or model settings?
- **Starting data/tools:** A small ONT dataset with raw signal and a trusted reference; [Dorado](https://github.com/nanoporetech/dorado). Match the model to the sequencing chemistry.
- **Initial framing:** Compare two supported configurations and characterize substitutions and indels in homopolymers, repeats, and other contexts, alongside runtime.
- **Optional extension:** Adapt a model or develop a targeted correction for one systematic error class.

### 2. Nanopore modification detection and signal alignment

- **Question:** How do signal alignment, coverage, and confidence thresholds affect modification detection?
- **Starting data/tools:** ONT signal or modification-tagged BAMs with appropriate controls; [Uncalled4](https://github.com/skovaka/uncalled4), [Dorado modification calling](https://software-docs.nanoporetech.com/dorado/latest/basecaller/mods/), and [Modkit](https://nanoporetech.github.io/modkit/).
- **Initial framing:** Evaluate one modification in selected regions using controls or an independent assay, and quantify coverage and threshold effects.
- **Optional extension:** Study allele-specific methylation or compare DNA and direct-RNA signal analysis. PacBio kinetics would require a separate platform-specific approach.

### 3. Detect and characterize tandem repeat expansions

- **Question:** Which repeat structures and read lengths make expansion detection difficult?
- **Starting data/tools:** Public long reads or simulated reads for selected repeat loci; [NanoSatellite](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1856-3) as a signal-level starting point.
- **Initial framing:** Evaluate repeat-length estimates at a small locus set, varying coverage or repeat complexity. Confirm raw-signal availability if using a signal-based method.
- **Optional extension:** Detect interruptions or develop a signal-level feature that improves estimates at one difficult locus.

### 4. Simulate adaptive sampling for targeted sequencing or assembly

- **Question:** Which read acceptance strategy best improves target coverage or assembly completeness under a fixed sequencing-time budget?
- **Starting data/tools:** Existing nanopore reads, offline simulation or replay, and approaches from [UNCALLED](https://www.nature.com/articles/s41587-020-0731-9), [Readfish](https://www.nature.com/articles/s41587-020-00746-x), or [BOSS-RUNS](https://www.biorxiv.org/content/10.1101/2020.02.07.938670v3).
- **Initial framing:** Compare a targeting or assembly-aware strategy with random acceptance. Model read rejection time and report enrichment, coverage, or assembly gains.
- **Optional extension:** Incorporate changing coverage, unknown species, or difficult genomic regions. A live sequencing experiment is optional.

## Genome Assembly, Pangenomes, and Variant Calling

### 5. Genome profiling and assembly at limited coverage

- **Question:** How do read accuracy and coverage affect genome-size estimates, heterozygosity estimates, and assembly quality?
- **Starting data/tools:** One small genome or an existing human assembly experiment; [GenomeScope2](https://www.nature.com/articles/s41467-020-14998-3), [Smudgeplot](https://github.com/KamilSJaron/smudgeplot), and an existing long-read assembler.
- **Initial framing:** Downsample reads and compare estimates and assembly results with a trusted reference. Treat accurate HiFi and noisier ONT reads separately.
- **Optional extension:** Investigate polyploidy or haplotype collapse using simulated data or a small plant genome; reuse existing overlap and assembly components.

### 6. Structural variant calling across difficult genomic contexts

- **Question:** Which SV types and genomic contexts explain differences between callers?
- **Starting data/tools:** [GIAB](https://github.com/genome-in-a-bottle) reads and truth sets; two compatible long-read SV callers and a consistent evaluation framework.
- **Initial framing:** Report precision and recall stratified by variant size, repeat context, and coverage within benchmark regions, rather than only an overall F1 score.
- **Optional extension:** Build a confidence classifier with held-out evaluation, or investigate how known-SV catalogs improve genotyping.

### 7. Does a pangenome reduce reference bias?

- **Question:** How does mapping to a graph rather than a linear reference affect allelic balance and variant accuracy?
- **Starting data/tools:** Existing [HPRC Release 2](https://humanpangenome.org/hprc-data-release-2/) resources, a graph mapper such as Giraffe, a linear mapper, and GIAB benchmarks. See the [draft human pangenome paper](https://www.nature.com/articles/s41586-023-05896-x).
- **Initial framing:** Compare selected loci or a small sample set for mapping ambiguity, allelic balance, and variant errors, using consistent coordinates and evaluation regions.
- **Optional extension:** Vary graph composition or assess performance in samples excluded from graph construction.

### 8. Discover variation absent from a single reference

- **Question:** What sequences or gene copies are missed when variation is described relative to one genome?
- **Starting data/tools:** A small set of existing HPRC assemblies, or assemblies from several varieties of a non-human species; [HPRC assembly resources](https://github.com/human-pangenomics/hprc_intermediate_assembly).
- **Initial framing:** Analyze one chromosome, gene family, or region to identify and characterize non-reference sequence or copy-number differences.
- **Optional extension:** Compare graph and k-mer representations, or test MinHash sketches as a fast screening method using [Mash](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-016-0997-x).

### 9. Evaluate assemblies and variants against a complete diploid benchmark

- **Question:** Which errors become apparent when benchmarking against a complete, phased diploid genome?
- **Starting data/tools:** [A complete diploid human genome benchmark for personalized genomics (2026)](https://pubmed.ncbi.nlm.nih.gov/42561913/), associated public resources, and selected assembly or variant outputs.
- **Initial framing:** Evaluate one error class in selected repeats, duplicated genes, or other difficult regions. For assembly projects, distinguish structural correctness, consensus accuracy, completeness, and phasing accuracy.
- **Optional extension:** Develop an error detector or compare conclusions obtained with older benchmark regions. [QUAST](https://quast.sourceforge.net/) and [Merqury](https://github.com/marbl/merqury) provide complementary assembly evaluation starting points.

### 10. Tumor purity and coverage in long-read somatic SV detection

- **Question:** How do tumor purity and sequencing depth affect detection of somatic rearrangements?
- **Starting data/tools:** Public tumor/normal cell-line datasets and available truth sets; [Severus](https://www.nature.com/articles/s41587-025-02618-8) and [PacBio demo datasets and workflow](https://github.com/PacificBiosciences/HiFi-somatic-WDL).
- **Initial framing:** Mix and downsample a tumor/normal pair and evaluate detection across purity and coverage settings. Begin with selected regions and distinguish somatic from germline calls.
- **Optional extension:** Compare callers or characterize complex rearrangements and breakpoint graphs.

## Functional Genomics and Genomic Machine Learning

### 11. Personalized references for allele-specific expression

- **Question:** How much does reference choice affect RNA-seq mapping and allele-specific expression estimates?
- **Starting data/tools:** Matched genotype and expression data from [MAGE](https://www.nature.com/articles/s41586-024-07708-2), a personalized or phased reference, and a conventional reference pipeline; [Allele-Seq](https://doi.org/10.1038/msb.2011.54) as background.
- **Initial framing:** Compare selected genes or samples for mapping bias and allelic imbalance, accounting for phasing availability and mapping ambiguity.
- **Optional extension:** Incorporate population variants through a graph reference or investigate variant detection from RNA-seq with controls for expression and alignment artifacts.

### 12. How much sequencing is needed to recover transcript isoforms?

- **Question:** How robust are long-read transcript discovery and quantification to reduced sequencing depth?
- **Starting data/tools:** One or two [SG-NEx](https://github.com/GoekeLab/sg-nex-data) or [LRGASP](https://www.nature.com/articles/s41592-024-02298-3) samples and two transcript-analysis tools. See the [2025 SG-NEx benchmark](https://www.nature.com/articles/s41592-025-02623-4).
- **Initial framing:** Downsample reads and compare isoform recovery, abundance stability, and recovery of spike-in controls where available. Distinguish unsupported novel calls from verified false positives.
- **Optional extension:** Study platform differences or allele-specific isoforms.

### 13. Chromatin-state annotation across genomes or species

- **Question:** How stable are chromatin-state annotations across reference choices, samples, or species?
- **Starting data/tools:** A compatible set of histone-mark tracks and [ChromHMM](https://www.nature.com/articles/nmeth.1906) or [Segway](https://segway.hoffmanlab.org/).
- **Initial framing:** Compare state assignments and their relationships to genes and regulatory elements in a restricted region or dataset, accounting for coordinate and assay differences.
- **Optional extension:** Develop a reproducible method to assign biological labels to states, or compare a phased reference with a standard reference.

### 14. Chromatin accessibility prediction under assay bias and dataset shift

- **Question:** How do assay bias correction and cell-type differences affect sequence-based accessibility predictions?
- **Starting data/tools:** A small [ENCODE](https://www.encodeproject.org/) ATAC-seq or DNase-seq dataset and pretrained [ChromBPNet](https://github.com/kundajelab/chrombpnet) models. ChromBPNet predicts accessibility profiles; it does not assign ChromHMM/Segway-style chromatin states.
- **Initial framing:** Evaluate predictions on held-out chromosomes or samples and examine how bias correction changes motif interpretation.
- **Optional extension:** Adapt a model to another cell type or test allele-specific regulatory predictions.

### 15. Do single-cell foundation models transfer across studies?

- **Question:** Do pretrained cell embeddings improve annotation or clustering compared with simpler representations?
- **Starting data/tools:** Two compatible public studies from [CELLxGENE](https://cellxgene.cziscience.com/), scGPT or Geneformer, and PCA with a conventional classifier. See [recent evaluation of single-cell foundation models](https://www.nature.com/articles/s41592-026-03120-y).
- **Initial framing:** Hold out a study or donors and compare cell-type accuracy and robustness to reduced counts. Distinguish frozen-model evaluation from fine-tuning and quantify compute costs.
- **Optional extension:** Evaluate rare or unseen cell types, batch integration, or trajectory stability. A conventional single-cell pipeline comparison is also a suitable project without a foundation model.

### 16. When do DNA foundation models beat simple baselines?

- **Question:** Do pretrained DNA embeddings improve a specific sequence classification or prediction task?
- **Starting data/tools:** A public labeled sequence dataset, two available pretrained models, and a k-mer classifier or small CNN; [Benchmarking DNA foundation models for genomic and genetic tasks (2025)](https://www.nature.com/articles/s41467-025-65823-8).
- **Initial framing:** Compare frozen embeddings and simple baselines on one task using chromosome or sequence-similarity splits, and report accuracy and resource use.
- **Optional extension:** Fine-tune a small model or analyze effects of sequence length, reverse complements, and species transfer.

### 17. Predict cell responses to perturbations

- **Question:** Can a model predict held-out perturbation effects better than simple average-response or linear baselines?
- **Starting data/tools:** The [CM4AI AnVIL Cell Map Prediction DREAM Challenge](https://www.synapse.org/Synapse:syn77349038) and its provided data and evaluation framework.
- **Initial framing:** Reproduce a baseline and evaluate one focused improvement, with a reproducible class report by December.
- **Optional extension:** Continue toward an official challenge submission. The [announced challenge schedule](https://anvilproject.dev.clevercanary.com/events/cm4ai-anvil2026-dream-challenge) runs from October 19, 2026 to February 1, 2027, beyond the course; confirm data availability and define a class-sized milestone before committing.

## Evolution, Disease, and Metagenomics

### 18. Evaluate regulatory variant predictions

- **Question:** How accurately do sequence-based models predict the direction and magnitude of non-coding variant effects?
- **Starting data/tools:** One public MPRA or QTL dataset, [AlphaGenome (2026)](https://www.nature.com/articles/s41586-025-10014-0), and a task-specific predictor. [CADD](https://cadd.gs.washington.edu/) can provide a complementary prioritization baseline where appropriate.
- **Initial framing:** Evaluate effect direction, ranking, and performance by distance to a gene or sequence context. Use inference or available predictions and confirm model access and compute needs early.
- **Optional extension:** Compare modalities or investigate disagreements at selected loci. Keep molecular-effect prediction distinct from disease-risk prediction.

### 19. Metagenomics with incomplete reference databases

- **Question:** How do missing species and closely related organisms affect classification and abundance estimates?
- **Starting data/tools:** A simulated community or a public mock community with known composition; compare methods such as [Kraken2](https://github.com/DerrickWood/kraken2), [Centrifuge](https://genome.cshlp.org/content/26/12/1721.full), or [pseudoalignment](https://doi.org/10.1093/bioinformatics/btx106).
- **Initial framing:** Remove selected organisms from the reference database and quantify classification errors, unassigned reads, and abundance bias, alongside memory and runtime.
- **Optional extension:** Add a learned classifier or unknown-species detector and test on held-out taxa rather than random reads from familiar genomes.

### 20. Identify species in food or environmental samples

- **Question:** Which species can be detected reliably in mixed samples, and how do contaminants affect conclusions?
- **Starting data/tools:** Public food or environmental sequencing data, matched controls where available, and an established classifier; [AllFoodSeq](https://doi.org/10.1093/bioinformatics/btw822) as background.
- **Initial framing:** Analyze a small sample set, evaluate detection thresholds with simulated mixtures or controls, and explain uncertain assignments.
- **Optional extension:** Compare short and long reads or investigate a biological association with appropriate covariate controls.

### 21. Population structure and sensitivity to reference panels

- **Question:** How do sample composition, marker choice, and filtering affect inferred population structure and heterozygosity?
- **Starting data/tools:** Public [1000 Genomes](https://www.internationalgenome.org/data/) or [SGDP](https://www.nature.com/articles/nature18964) variants; PCA, admixture analysis, and standard diversity statistics.
- **Initial framing:** Compare balanced and imbalanced panels or marker subsets, and quantify the stability of inferred structure or diversity estimates. Interpret these as genetic patterns rather than predictions of ethnicity.
- **Optional extension:** Evaluate local ancestry on simulated admixed genomes or investigate sensitivity to variant ascertainment.

## CS Theory and Systems

### 22. Learned indexes and scalable sequence search

- **Question:** Can a learned index, sketch, or compact data structure improve a well-defined genomic search workload?
- **Starting data/tools:** A small collection of genomes or sequencing datasets; [learned indexes](https://arxiv.org/abs/1712.01208), [Sequence Bloom Trees](https://www.nature.com/articles/nbt.3442), or [Mantis](https://www.cell.com/cell-systems/fulltext/S2405-4712(18)30239-4).
- **Initial framing:** Compare with an established index on build time, query time, memory, and correctness or false-positive rate using a fixed query set.
- **Optional extension:** Test scaling, incremental updates, or an application to detecting non-reference sequence.

### 23. Accelerate a genomics bottleneck

- **Question:** Which computational bottleneck limits an analysis, and how much can targeted optimization improve it?
- **Starting data/tools:** A reproducible pipeline and a small input dataset; CPU parallelism, GPUs, or cloud execution as appropriate.
- **Initial framing:** Profile the baseline, optimize one component, verify equivalent outputs, and measure end-to-end time, memory, and cost where relevant.
- **Optional extension:** Use the improvement to analyze a larger dataset or compare scaling across hardware configurations.

### 24. Browser-based genomics and interactive visualization

- **Question:** Can an interactive tool make a specific genomic analysis easier to perform or interpret?
- **Starting data/tools:** A bounded task such as FASTQ quality summaries, assembly-graph exploration, SV comparison, or single-cell annotation review; WebAssembly or standard browser visualization libraries.
- **Initial framing:** Build a usable prototype with representative data, compare performance or usability with an existing workflow, and demonstrate a biological or computational insight.
- **Optional extension:** Support larger files, streaming computation, or linked views across multiple data types.

### 25. Compression and selective access to sequencing data

- **Question:** Can sequencing data be stored more compactly while preserving efficient access for a defined analysis?
- **Starting data/tools:** Small short-read and long-read FASTQ/BAM datasets and existing compression formats as baselines.
- **Initial framing:** Compare size, encoding/decoding time, memory, and round-trip correctness for a proposed improvement. Include reference or dictionary storage in the total size.
- **Optional extension:** Add region-based access or study lossy quality-score compression with downstream accuracy evaluation.

### 26. What do CNN filters learn in natural images versus genomic pileups?

- **Question:** How do the convolutional filters learned by AlexNet on natural images compare with those learned by DeepVariant on sequencing read pileups? Which patterns do they recognize, and how do those patterns relate to their classification tasks?
- **Starting data/tools:** A pretrained [AlexNet](https://www.cs.toronto.edu/~kriz/imagenet_classification_with_deep_convolutional.pdf), a small set of natural images, and a released [DeepVariant](https://github.com/google/deepvariant) model with a small set of pileup examples. The [pileup visualization guide](https://github.com/google/deepvariant/blob/r1.10/docs/show-examples.md) and [Looking Through DeepVariant's Eyes](https://google.github.io/deepvariant/posts/2020-02-20-looking-through-deepvariants-eyes/) explain the genomic inputs. Record the model versions and architectures: DeepVariant uses an Inception-based CNN rather than AlexNet, and its input channels encode sequencing features rather than RGB colors.
- **Initial framing:** Visualize first-layer filter weights and activation maps from both models, then inspect examples that strongly activate selected filters. Compare patterns such as edges and textures in natural images with read/reference mismatches, allele support, and quality information in pileups. Explain each input channel and account for differences in filter size and architecture when interpreting the comparison. Use pretrained models so the focus is on understanding what they learned.
- **Optional extension:** Test whether the identified features matter by masking input channels or regions and measuring changes in predictions, using unmodified examples as controls. Compare an ImageNet-trained Inception model with DeepVariant to better separate architecture effects from training-domain effects, or examine how features change across layers or sequencing technologies. See [DeepVariant channel-ablation experiments](https://google.github.io/deepvariant/posts/2024-09-04-blindfolding-deepvariant-surprising-insights-from-hiding-information/) for inspiration.

## Or Your Own Idea!

Your project can build on a dataset or idea from your research, but should include a new question or contribution beyond work you are already doing for your PhD. If you have a research idea but need data, let me know and I will help you find a suitable starting point.

## Data Resources

Use a specific release and record the sample identifiers, download links, reference genome, and access requirements. Check that the required data files—not just the paper—are available before choosing a project.

### Public Human Genomes and Benchmarks

- [1000 Genomes](https://www.internationalgenome.org/data/): Public population variation and sequencing resources; see the [expanded sequencing study](https://pubmed.ncbi.nlm.nih.gov/36055201/).
- [Simons Genome Diversity Project (SGDP)](https://www.nature.com/articles/nature18964): Sequencing resources for studying human genetic diversity.
- [Human Pangenome Reference Consortium Release 2](https://humanpangenome.org/hprc-data-release-2/): Long-read sequencing, phased assemblies, and associated resources; consult release-specific manifests for available samples and files.
- [T2T CHM13](https://github.com/marbl/CHM13): Complete reference assembly and associated analysis resources.
- [Genome in a Bottle (GIAB)](https://github.com/genome-in-a-bottle): Sequencing data, variant truth sets, and benchmark regions. Use truth sets and regions matched to the sample and reference.
- [Complete diploid human genome benchmark (2026)](https://pubmed.ncbi.nlm.nih.gov/42561913/): A starting point for evaluating personalized genomes and difficult regions; follow the paper's data availability links.
- [Public tumor/normal HiFi datasets](https://github.com/PacificBiosciences/HiFi-somatic-WDL): COLO829 and HCC1395 demo datasets with workflow and benchmark pointers.
- [Personal Genome Project](https://www.personalgenomes.org/): Public participant-contributed genomic and phenotype resources; check the contents and usage terms of individual datasets.

### Expression, Epigenomics, and Single-Cell Data

- [MAGE](https://www.nature.com/articles/s41586-024-07708-2): Expression data from 731 samples from the 1000 Genomes Project, useful for genotype/expression analyses.
- [ENCODE](https://www.encodeproject.org/): Functional genomics assays, processed tracks, and metadata.
- [GTEx](https://gtexportal.org/home/): Public processed expression and QTL resources across tissues; individual-level genomic and raw sequencing data have controlled access requirements.
- [SG-NEx](https://github.com/GoekeLab/sg-nex-data): Long-read and short-read RNA-seq resources with protocol comparisons and controls.
- [LRGASP](https://www.nature.com/articles/s41592-024-02298-3): Long-read transcript identification and quantification benchmark; follow the data availability links for datasets and evaluation resources.
- [CELLxGENE Discover](https://cellxgene.cziscience.com/) and [Census](https://chanzuckerberg.github.io/cellxgene-census/index): Public single-cell datasets and programmatic access. Record the release and avoid counting duplicate cells across aggregated studies.
- [CM4AI DREAM Challenge](https://www.synapse.org/Synapse:syn77349038): Check challenge registration, data access, and evaluation rules; its final deadline falls after the course.

### Cancer Resources and Data Requiring Registration or Controlled Access

- [ICGC Data Portal retirement notice](https://docs.icgc.org/index.html): The original portal closed in June 2024. Follow the notice to successor resources and check access requirements for each dataset.
- [All of Us Researcher Workbench](https://www.researchallofus.org/data-tools/workbench/): Genomic, EHR, and other participant data requiring institutional eligibility, registration, training, and appropriate tier access. Choose this resource only if access is available in time for the project.

### Non-Human and Simulated Data

- [Darwin's Ark study](https://www.science.org/doi/10.1126/science.abk0639) and [dog genomics data](https://data.broadinstitute.org/DogData/): Dog genotype and phenotype resources for comparative and population analyses.
- Simulated reads, communities, variants, or expression data are suitable when they address a clear question. Report simulation assumptions and, where feasible, include a small real-data validation.
