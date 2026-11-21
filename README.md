
# RNA-Seq Guided Replication Using Galaxy and GitHub

## Group 4 – Salinity Stress

### Group Members
- Balana, Jane Kate
- Catipay, Nina Karylle
- Itom, Rachelle May
- Olaguir, Junavhel Jane
- Palopalo, Hobe
- Trayvilla, Shekhan Fredianne

---

## 1. Selected Research Study

**Title:** *The impact of salt stress on the physiology and the transcriptome of the model streptophyte green alga Chara braunii*  
**Authors:** Daniel Heß, Carolin M. Heise, Hendrik Schubert, Wolfgang R. Hess, and Martin Hagemann  
**Year:** 2023  
**Journal:** *Physiologia Plantarum*, 175(6), e14123  
**Organism:** *Chara braunii*  
**Tissue/cell type:** Subsurface thalli segments  
**Assigned topic:** Salinity stress  
**Research paper:** https://docs.google.com/document/d/1DDHZhxmksA6TEZPLP_722WIIIyYJnilHWdJjMDFZ9OI/edit?tab=t.0

### Research question
How does exposure to 5 PSU salt stress for 24 hours affect gene expression in *Chara braunii* compared with freshwater control conditions?

### Background and rationale
Salinity stress is an important abiotic challenge that can affect cellular homeostasis, ion regulation, and other physiological processes. *Chara braunii* is a streptophyte green alga used to investigate responses to environmental stress and evolutionary relationships with land plants. Heß et al. (2023) examined the physiology and transcriptome of *C. braunii* exposed to salt stress. This group re-analyzed a subset of the study's public RNA-seq data using Galaxy to examine differential expression and compare the results with the published study.

## 2. Organism and Experimental Design

| Item | Information |
|---|---|
| Organism | *Chara braunii* |
| Tissue/cell type | Subsurface thalli segments |
| Control | Freshwater, 0 PSU |
| Treatment | 5 PSU salt stress |
| Treatment duration analyzed | 24 hours |
| Biological replicates | 2 per condition |
| Sequencing | Paired-end, 2 × 100 bp (as reported for the original study) |
| Sequencing protocol | Illumina TruSeq stranded protocol |
| Sequencing facility (original study) | Cologne Center for Genomics |

## 3. RNA-seq Dataset and Accessions

The raw reads were retrieved from the NCBI Sequence Read Archive (SRA), BioProject **PRJNA965918**.

| Sample | Run accession | Condition |
|---|---|---|
| Control replicate 1 | SRR24401889 | 0 PSU freshwater |
| Control replicate 2 | SRR24401893 | 0 PSU freshwater |
| Treatment replicate 1 | SRR24401886 | 5 PSU, 24 hours |
| Treatment replicate 2 | SRR24401892 | 5 PSU, 24 hours |

The group analyzed four runs: two control replicates and two 24-hour salt-treatment replicates. The original study included additional time points, but these were not included in this re-analysis.

## 4. Reference Genome and Annotation

| Item | Group information |
|---|---|
| Genome | *Chara braunii* S276 |
| Assembly | Cbr_1.0 |
| Assembly accession | GCA_003427395.1 |
| Genome format | FASTA |
| Annotation used by group | NCBI genome annotation |
| Annotation format | GTF |
| Annotation used by original authors | OrcAE gene model / GTF (as described in the report) |

The group used the Cbr_1.0 reference genome and an NCBI GTF annotation. The original study used the OrcAE gene model. This difference in annotation source is a methodological difference that may affect gene-level quantification and comparison of results.

## 5. Original Authors' RNA-seq Pipeline

The original study's workflow, as summarized in the group's methods review, included:

1. FastQC and MultiQC for read-quality assessment.
2. Cutadapt for adapter and barcode removal.
3. STAR for read alignment to the reference genome.
4. STAR per-gene read counting using the OrcAE GTF gene model.
5. DESeq2 for differential expression analysis.
6. goseq for Gene Ontology analysis.

### Original study parameters recorded by the group

- **Read type:** Paired-end, 2 × 100 bp.
- **Cutadapt:** quality cutoff 20; minimum overlap 3 bp; maximum error rate 0.1; adapter match times 1; minimum length reported as 20 bp for R1 in the group's method summary.
- **STAR:** splice-junction sequence length 100 bp; SA pre-indexing string 14; 2-pass mapping disabled.
- **DEG criterion in the original study:** adjusted p-value ≤ 0.05 and absolute log2 fold change ≥ 1, as recorded in the group's report.

## 6. Galaxy Pipeline Used by the Group

The group used Galaxy to process and analyze the selected paired-end RNA-seq data.

**Pipeline:**

Raw paired-end reads → FastQC → Cutadapt → STAR alignment → STAR GeneCounts → DESeq2 → result interpretation

| Analysis step | Tool used |
|---|---|
| Raw read quality control | FastQC |
| Adapter trimming and filtering | Cutadapt |
| Genome alignment | RNA STAR |
| Gene-level read counting | STAR GeneCounts |
| Differential expression | DESeq2 |

The group documented the processing steps through its Galaxy history and output reports.

## 7. Quality Control (FastQC)

FastQC was used to assess raw paired-end reads before trimming. All four samples passed the general sequence quality assessment reported by the group. However, some files showed warnings or failures for adapter contamination, overrepresented sequences, per-base sequence content, sequence duplication levels, and per-sequence GC content. Poly-G sequences were also reported among overrepresented sequences in some reverse-read files.

These observations indicated that read trimming and filtering were appropriate before downstream analysis. FastQC observations from the group's report are summarized below.

| Sample | General quality | QC observations recorded |
|---|---|---|
| SRR24401893 | R1 and R2: PASS | R2 adapter WARN and overrepresented sequences FAIL; sequence duplication and per-base sequence content concerns were reported |
| SRR24401889 | R1 and R2: PASS | R2 adapter WARN, overrepresented sequences FAIL; sequence duplication, per-base sequence content, GC-content concerns and Poly-G noted |
| SRR24401886 | R1 and R2: PASS | Overrepresented sequences WARN/FAIL; per-base sequence content and duplication concerns; GC-content WARN on R2 |
| SRR24401892 | R1 and R2: PASS | Overrepresented sequences WARN/FAIL; per-base sequence content and duplication concerns; GC-content WARN on R2 |

### FastQC evidence

![FastQC screenshot](FastQC%Results.png)

## 8. Read Trimming (Cutadapt)

Cutadapt was used to remove adapter sequences and filter reads prior to alignment. After trimming, the read counts reported in the group's table were:

| SRA accession | Read pairs reported after trimming (R1) | Read pairs reported after trimming (R2) |
|---|---:|---:|
| SRR24401886 | 71,140,169 | 71,140,169 |
| SRR24401889 | 56,282,910 | 56,282,910 |
| SRR24401892 | 45,288,581 | 45,288,581 |
| SRR24401893 | 44,262,281 | 44,262,281 |

The number of reads decreased after trimming due to the removal of adapters and low-quality sequences. The report for SRR24401886 showed 72,004,032 read pairs processed and 71,140,169 pairs written after filtering (98.8%).

### Cutadapt evidence

![Cutadapt report](Cutadapt_Report.png)

## 9. Genome Mapping (STAR)

STAR was used to align the trimmed reads to the *Chara braunii* Cbr_1.0 reference genome.

| SRA accession | Total reads reported | Mapped | Uniquely mapped | Unmapped: too short |
|---|---:|---:|---:|---:|
| SRR24401886 | 71,140,169 | 83.50% | 79.76% | 16.48% |
| SRR24401889 | 56,282,910 | 82.20% | 79.11% | 17.78% |
| SRR24401892 | 45,288,581 | 79.26% | 75.28% | 20.71% |
| SRR24401893 | 44,262,281 | 84.56% | 81.92% | 15.42% |

Mapping percentages ranged from 79.26% to 84.56%. The uniquely mapped proportion ranged from 75.28% to 81.92%. SRR24401892 had the lowest reported overall mapping percentage and the highest proportion of reads reported as unmapped because they were too short. Overall, the trimmed reads were aligned to the reference genome.

### STAR mapping evidence (representative sample: SRR24401893)

![STAR mapping report](STAR_Mapping.png)

## 10. Gene Counting (STAR GeneCounts)

Gene-level read counts were generated using STAR GeneCounts. The count tables from the four samples were combined into a table in which rows represented genes and columns represented samples. These count data were used as input for DESeq2.

### Example of gene counts reported in the group's count table

| Gene | SRR24401886 | SRR24401889 | SRR24401892 | SRR24401893 |
|---|---:|---:|---:|---:|
| CBR_g1 | 106 | 149 | 126 | 90 |
| CBR_g2 | 1 | 0 | 0 | 0 |
| CBR_g3 | 2,112 | 856 | 622 | 806 |
| CBR_g4 | 2,380 | 1,347 | 599 | 1,290 |
| CBR_g5 | 0 | 0 | 0 | 0 |
| CBR_g6 | 0 | 0 | 0 | 0 |
| CBR_g7 | 11,453 | 7,131 | 7,361 | 7,840 |

The table above shows a small excerpt of the gene-count table documented in the report, rather than the complete dataset.

## 11. Differential Gene Expression (DESeq2)

DESeq2 was used to compare the freshwater control samples (0 PSU) with the salt-treated samples (5 PSU, 24 hours).

| Metric | Result reported by group |
|---|---:|
| Gene features in count table | 35,420 |
| Genes with valid p-values | 18,764 |
| Genes with adjusted p-values available | 12,867 |
| Significant differentially expressed genes reported | 174 |
| Positive log2 fold change | 69 |
| Negative log2 fold change | 105 |
| Adjusted p-value / FDR threshold | 0.05 |

The group reported 174 significant gene features at the adjusted p-value threshold of 0.05: 69 with positive log2 fold-change values and 105 with negative values. Positive log2 fold change indicates higher expression in the treatment relative to the control for the stated comparison, and negative log2 fold change indicates lower expression.

### DESeq2 result table

![DESeq2 results](DESeq2_Results.png)

### MA plot

The MA plot displays the relationship between normalized mean counts and log2 fold change for the control-versus-treatment comparison.

![MA plot](MA_Plot.png)

## 12. Selected Differentially Expressed Genes

Three genes were selected from the DESeq2 results based on their fold-change values and adjusted p-values.

| Gene ID | Log2 fold change | Adjusted p-value | Direction |
|---|---:|---:|---|
| CBR_g17758 | +4.6674 | 1.5285 × 10⁻²² | Upregulated |
| CBR_g25795 | +4.5073 | 1.5285 × 10⁻²² | Upregulated |
| CBR_g770 | −3.1771 | 4.6744 × 10⁻¹⁴ | Downregulated |

### Functional annotation and possible relationship to salinity stress

| Gene ID | Functional annotation reported | Possible relationship to salinity stress |
|---|---|---|
| CBR_g17758 | Hypothetical protein; no conserved domain identified | Its specific function and possible role in salinity response remain unknown. |
| CBR_g25795 | Hypothetical protein containing a DUF4360 domain of unknown function | Its upregulation suggests a response to salt treatment, but its specific biological role remains undetermined. |
| CBR_g770 | Hypothetical protein; no conserved domain identified | Its downregulation indicates reduced expression under salt treatment, but its specific role remains unknown. |

CBR_g17758 and CBR_g25795 had positive log2 fold-change values, indicating higher expression under the treatment condition. CBR_g770 had a negative log2 fold-change value, indicating lower expression under treatment. All three genes had adjusted p-values below 0.05. However, their specific functions and mechanisms in salinity stress remain unclear because of limited functional annotations.

## 13. Comparison with the Original Study

### Overall comparison

The group's analysis partially recovered the biological response described by the original study. The DESeq2 results identified genes that were significantly differentially expressed under the 5 PSU salinity treatment compared with the control. However, the selected genes did not clearly reproduce the major functional responses reported in the original study, particularly increased expression of genes associated with ion transport and cell-wall modification.

### Comparison observations

- **Shared response:** Both analyses identified gene-expression changes following exposure to 5 PSU salinity.
- **Gene also discussed in the original paper:** CBR_g770 was discussed in the original study as a putative stress-enhanced protein 2-like gene.
- **Expression-direction discrepancy:** The original study reported increased expression of CBR_g770 after 24 hours of salt stress, whereas the group's analysis showed decreased expression (log2 fold change = −3.1771).
- **Other selected genes:** CBR_g17758 and CBR_g25795 were not among the specifically highlighted genes discussed in the main text of the original paper.
- **Software:** The group used FastQC, Cutadapt, RNA STAR, and DESeq2, also used in the original analysis; software versions and computational environments may have differed.
- **Reference:** The group used the same Cbr_1.0 genome assembly (GCA_003427395.1), but an NCBI annotation rather than the OrcAE annotation used by the original authors.
- **Sample subset:** The group analyzed four runs (two controls and two 24-hour salt-treated samples), while the original study included additional time points, including 6 and 48 hours.

### Part 14 comparison table

| Analysis step | Original authors | Group re-analysis |
|---|---|---|
| RNA-seq dataset | *Chara braunii* S276; control and 5 PSU samples at 6, 24, and 48 hours with biological replicates | Four selected runs: two controls (0 PSU) and two treatments (5 PSU, 24 hours) |
| Quality control | FastQC and MultiQC | FastQC |
| Trimming | Cutadapt | Cutadapt |
| Reference genome | Published *C. braunii* genome, Cbr_1.0 | Cbr_1.0 (GCA_003427395.1) |
| Annotation | OrcAE GTF gene model | NCBI GTF annotation for Cbr_1.0 |
| RNA-seq aligner | RNA STAR | RNA STAR |
| Gene counting | STAR per-gene read counts | STAR per-gene read counts |
| Differential expression | DESeq2 | DESeq2 |
| Significance threshold | Adjusted p-value ≤ 0.05 and absolute log2 fold change ≥ 1 | Adjusted p-value ≤ 0.05 reported; selected genes also had absolute log2 fold change ≥ 1 |
| Main genes/pathways identified | Ion and solute transport, cell-wall synthesis/modification, photosynthesis, and carbon metabolism | Selected genes: CBR_g17758, CBR_g25795, and CBR_g770 |

### Why results may differ

Differences in software versions, analysis parameters, reference annotations, and sample selection could contribute to differences between the two analyses. Analyzing only a subset of the original samples may also limit the number and variety of differentially expressed genes detected. Annotation differences can affect gene-level assignment and quantification.

## 14. Limitations

- Only four RNA-seq samples were included, with two biological replicates per condition.
- The group analyzed a subset of the original dataset, not all time points.
- The group used NCBI GTF annotation, whereas the original study used OrcAE annotation.
- Software versions and computational environments may have differed.
- The re-analysis did not include the original study's Gene Ontology enrichment analysis with goseq.
- The selected genes had limited functional annotation, restricting biological interpretation.
- The reported mapping and expression results may be affected by differences in parameters, reference files, annotation, and sample selection.

## 15. Conclusion

The RNA-seq re-analysis of *Chara braunii* under salinity stress demonstrated how public sequencing data can be processed and analyzed using Galaxy to investigate gene-expression changes. The group performed quality control, read trimming, genome alignment, gene counting, and differential expression analysis using a Galaxy-based pipeline. The analysis identified 174 significant gene features as reported at an adjusted p-value threshold of 0.05 and selected three genes for further interpretation.

The group partially recovered the transcriptional response to salt stress but did not clearly reproduce the major functional responses emphasized by the original study. The difference in the expression direction of CBR_g770 and the use of a different annotation source illustrate how methodological differences can affect RNA-seq re-analysis. The exercise emphasized the importance of transparent documentation, comparison with published results, and careful interpretation of transcriptomic findings.

## 16. Galaxy History and Workflow Evidence

The Galaxy history documents the sequence of datasets and tools used during the group's RNA-seq analysis. The history screenshot provides evidence of the processing steps performed in Galaxy.

![Galaxy History](Documented%Galaxy%History.png)


## 17. Student Contributions

| Group member | Contribution |
|---|---|
| Balana, Jane Kate | Galaxy, Paper |
| Catipay, Nina Karylle | Paper  |
| Itom, Rachelle May | Galaxy, Paper |
| Olaguir, Junavhel Jane | Galaxy, Paper |
| Palopalo, Hobe | Paper, Github |
| Trayvilla, Shekhan Fredianne | Galaxy, Paper  |

## 18. References

Heß, D., Heise, C. M., Schubert, H., Hess, W. R., & Hagemann, M. (2023). *The impact of salt stress on the physiology and the transcriptome of the model streptophyte green alga Chara braunii*. *Physiologia Plantarum*, 175(6), e14123. [Insert DOI/publisher link]

NCBI Sequence Read Archive (SRA). BioProject **PRJNA965918**.

NCBI Genome Assembly: **GCA_003427395.1**, *Chara braunii* S276, Cbr_1.0.

## 19. Repository Structure

```text
Assignment_RNASeq/
├── README.md
├── paper/
│   └── research-paper-link.txt
├── methods/
│   └── pipeline-summary.md
├── galaxy_workflow/
│   └── workflow-export.ga (if exported)
├── results/
│   ├── qc-summary.md
│   ├── mapping-summary.md
│   ├── gene-count-summary.md
│   └── differential-expression-summary.md
├── figures/
│   ├── fastqc/
│   │   └── FastQC_Results.png
│   ├── trimming/
│   │   └── Cutadapt_Report.png
│   ├── mapping/
│   │   └── STAR_Mapping.png
│   ├── gene-counts/
│   ├── differential-expression/
│   │   ├── DESeq2_Results.png
│   │   └── MA_Plot.png
│   ├── history/
│   │   └── Documented_Galaxy_History.png
└── student_contributions/
    └── contributions.md
```
