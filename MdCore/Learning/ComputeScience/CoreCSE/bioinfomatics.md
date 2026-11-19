# 25 · BIOINFORMATICS & COMPUTATIONAL BIOLOGY

## 25.1 Sequence Analysis

### Sequence Alignment

**Definition:** Arranging DNA, RNA, or protein sequences to identify regions of similarity that may indicate functional, structural, or evolutionary relationships.

**Why it matters:** Similar sequences often have similar functions. Alignment is the foundation of evolutionary biology, drug discovery, and clinical genomics.

**Types of alignment:**
- **Global (Needleman-Wunsch):** Align entire sequences end-to-end. Best for similar-length sequences.
- **Local (Smith-Waterman):** Find best-matching subsequences. Best for dissimilar sequences with conserved regions.
- **Semi-global:** Align one sequence entirely; the other partially.

**Scoring:**
- **Match:** Positive score (e.g., +1)
- **Mismatch:** Negative score (e.g., −1)
- **Gap penalty:** Negative (e.g., −2); often affine (gap open + gap extend)

**Needleman-Wunsch algorithm:**
```
Initialize matrix F[0..m][0..n]
F[i][0] = i × gap_penalty
F[0][j] = j × gap_penalty
For i = 1..m:
  For j = 1..n:
    match = F[i-1][j-1] + score(s[i], t[j])
    delete = F[i-1][j] + gap_penalty
    insert = F[i][j-1] + gap_penalty
    F[i][j] = max(match, delete, insert)
Traceback from F[m][n] to F[0][0]
```

**Complexity:** O(mn) time, O(mn) space (can be reduced to O(min(m,n)) with Hirschberg's algorithm).

**Smith-Waterman:** Same as NW but with 0 as minimum (local alignment); traceback starts from max score.

**BLAST (Basic Local Alignment Search Tool):** Heuristic; fast; finds local alignments by seeding. Used for database searches.

**Multiple sequence alignment (MSA):** Align >2 sequences. Tools: Clustal Omega, MUSCLE, MAFFT. NP-hard; heuristics used.

**Recall:** *Global = NW; Local = SW; BLAST = fast heuristic.*

### Genomics

**Definition:** Study of the genome — the complete set of DNA in an organism.

**Sequencing technologies:**
| Generation | Technology | Read length | Throughput | Examples |
|------------|-----------|-------------|------------|----------|
| 1st | Sanger | ~1000 bp | Low | ABI 3730 |
| 2nd | Illumina | 50–300 bp | Very high | HiSeq, NovaSeq |
| 2nd | Ion Torrent | 200–400 bp | High | Ion GeneStudio |
| 3rd | PacBio | 10–100 kb | Medium | Sequel, Revio |
| 3rd | Nanopore | 10–100+ kb | High | MinION, PromethION |

**Genome assembly:**
- **De novo:** Assemble without reference. Uses overlap-layout-consensus or de Bruijn graphs.
- **Reference-based:** Align reads to reference genome. Faster; requires reference.

**De Bruijn graph:** Nodes = k-mers; edges = overlaps. Used by SPAdes, Velvet.

**Variant calling:**
- **SNP (Single Nucleotide Polymorphism):** Single base change
- **Indel:** Insertion or deletion
- **CNV (Copy Number Variation):** Large duplications/deletions
- **SV (Structural Variant):** Large rearrangements

**Tools:** GATK, FreeBayes, Samtools, BCFtools.

**Recall:** *Sequencing, assembly, variant calling.*

### Transcriptomics

**Definition:** Study of RNA transcripts.

**RNA-seq:** Sequence RNA to measure gene expression.
- **Workflow:** Extract RNA → reverse transcribe → sequence → align to genome → count reads per gene
- **Differential expression:** DESeq2, edgeR, limma

**Single-cell RNA-seq (scRNA-seq):** Measure expression per cell.
- **Platforms:** 10x Genomics, Smart-seq
- **Analysis:** Seurat, Scanpy; clustering, trajectory inference

**Recall:** *RNA-seq, single-cell.*

### Epigenomics

**Definition:** Study of epigenetic modifications — changes that affect gene expression without changing DNA sequence.

**Modifications:**
- **DNA methylation:** Adding methyl groups (usually represses genes)
- **Histone modification:** Acetylation, methylation, phosphorylation
- **Chromatin accessibility:** Open vs closed regions

**Techniques:**
- **Bisulfite sequencing:** Methylation
- **ChIP-seq:** Protein-DNA binding
- **ATAC-seq:** Chromatin accessibility
- **Hi-C:** 3D genome organization

**Recall:** *Methylation, chromatin, 3D genome.*

### Metagenomics

**Definition:** Study of genetic material from environmental samples.

**Use cases:** Microbiome, environmental samples, infectious disease.

**Challenges:** Unknown species, low abundance, contamination.

**Tools:** QIIME, MEGAN, Kraken, MetaPhlAn.

**Recall:** *Microbiome analysis.*

---

## 25.2 Structural Biology

### Proteomics

**Definition:** Study of proteins — structure, function, interactions.

**Protein structure levels:**
- **Primary:** Amino acid sequence
- **Secondary:** α-helix, β-sheet
- **Tertiary:** 3D fold
- **Quaternary:** Multi-subunit assembly

**AlphaFold:** Deep learning model predicting protein structure from sequence.
- **AlphaFold 2:** CASP14 breakthrough; near-experimental accuracy
- **AlphaFold 3:** Extends to protein-ligand, protein-nucleic acid complexes
- **Open source:** Available via ColabFold, AlphaFold DB

**Traditional methods:**
- **X-ray crystallography:** High resolution; requires crystal
- **NMR:** Solution structure; small proteins
- **Cryo-EM:** Large complexes; Nobel 2017

**Recall:** *AlphaFold, X-ray, cryo-EM.*

### Molecular Dynamics

**Definition:** Simulate physical movements of atoms and molecules.

**Force fields:** AMBER, CHARMM, GROMOS, OPLS.

**Integration:** Verlet, leapfrog.

**Use:** Protein folding, drug binding, materials.

**Tools:** GROMACS, NAMD, AMBER, OpenMM.

**Recall:** *Simulate molecular motion.*

### Drug Discovery

**Pipeline:**
1. **Target identification:** Protein involved in disease
2. **Hit discovery:** Find molecules that bind target
3. **Lead optimization:** Improve potency, selectivity, ADMET
4. **Preclinical:** Animal testing
5. **Clinical trials:** Phase I, II, III
6. **Approval:** FDA, EMA

**Computational methods:**
- **Virtual screening:** Dock millions of compounds
- **QSAR:** Quantitative structure-activity relationship
- **De novo design:** Generate novel molecules
- **ADMET prediction:** Absorption, distribution, metabolism, excretion, toxicity

**Tools:** AutoDock, Schrödinger, RDKit, DeepChem.

**Recall:** *Target → hit → lead → trials.*

---

## 25.3 Evolutionary & Systems Biology

### Phylogenetics

**Definition:** Study of evolutionary relationships among organisms.

**Tree types:**
- **Rooted:** Has common ancestor
- **Unrooted:** No direction
- **Cladogram:** Branching only
- **Phylogram:** Branch lengths = changes

**Methods:**
- **Distance-based:** UPGMA, neighbor-joining
- **Character-based:** Maximum parsimony, maximum likelihood, Bayesian

**Tools:** RAxML, MrBayes, BEAST, MEGA.

**Recall:** *Evolutionary trees.*

### Systems Biology

**Definition:** Study of biological systems as integrated networks.

**Networks:**
- **Gene regulatory networks (GRN):** Genes regulating genes
- **Protein-protein interaction (PPI):** Physical interactions
- **Metabolic networks:** Biochemical reactions

**Modeling:** ODEs, Boolean networks, Bayesian networks.

**Tools:** Cytoscape, BioTapestry, CellDesigner.

**Recall:** *Network modeling.*

### Biological Networks

**Properties:**
- **Scale-free:** Few hubs, many low-degree nodes
- **Small-world:** Short paths, high clustering
- **Modular:** Functional modules

**Analysis:** Centrality, clustering, motif discovery.

**Recall:** *Hubs, small-world, modular.*

---

## 25.4 Clinical & Applied

### Clinical Genomics

**Definition:** Application of genomics to clinical care.

**Use cases:**
- **Diagnosis:** Rare genetic diseases
- **Pharmacogenomics:** Drug response based on genetics
- **Cancer genomics:** Tumor profiling, targeted therapy
- **Prenatal screening:** Non-invasive prenatal testing (NIPT)

**Challenges:** Variant interpretation, incidental findings, privacy.

**Recall:** *Genomics in medicine.*

### Variant Interpretation

**ACMG guidelines:** Pathogenic, likely pathogenic, uncertain, likely benign, benign.

**Databases:** ClinVar, gnomAD, OMIM, dbSNP.

**Tools:** VEP, SnpEff, ANNOVAR.

**Recall:** *Classify variants.*

### Pharmacogenomics

**Definition:** Study of how genes affect drug response.

**Examples:**
- **CYP2D6:** Metabolizes many drugs
- **TPMT:** Thiopurine toxicity
- **HLA-B*57:01:** Abacavir hypersensitivity

**Recall:** *Genes + drugs.*

### Cancer Genomics

**Definition:** Study of genetic changes in cancer.

**Key genes:**
- **Oncogenes:** Promote cancer (KRAS, MYC)
- **Tumor suppressors:** Inhibit cancer (TP53, BRCA1/2)
- **DNA repair:** Maintain genome (MLH1, MSH2)

**Analysis:** Mutational signatures, driver vs passenger mutations, tumor heterogeneity.

**Recall:** *Oncogenes, tumor suppressors.*

**SOTA:** AlphaFold 3, single-cell analysis, CRISPR design tools, scGPT foundation models for single-cell multi-omics.

---
