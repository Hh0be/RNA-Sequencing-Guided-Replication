
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

**Title:** The impact of salt stress on the physiology and the transcriptome of the model streptophyte green alga *Chara braunii*

**Authors:** Daniel Heß, Carolin M. Heise, Hendrik Schubert, Wolfgang R. Hess, and Martin Hagemann

**Year:** 2023

**Journal:** *Physiologia Plantarum*, 175(6), e14123

**Organism:** *Chara braunii*

**Assigned Topic:** Salinity Stress

---

## 2. Research Question

How does exposure to 5 PSU salt stress for 24 hours affect gene expression in *Chara braunii* compared with freshwater control conditions?

---

## 3. Organism and Experimental Conditions

The organism investigated in this study is *Chara braunii*, a streptophyte green alga used as a model for understanding physiological and molecular responses to environmental stress.

| Condition | Description |
|---|---|
| Control | Freshwater, 0 PSU |
| Treatment | 5 PSU salt stress |
| Treatment duration | 24 hours |
| Organism | *Chara braunii* |
| Tissue | Subsurface thalli segments |
| Replicates | 2 biological replicates per condition |

---

## 4. RNA-Seq Dataset

The RNA-seq data were obtained from the NCBI Sequence Read Archive (SRA).

**BioProject accession:** PRJNA965918

| Sample | SRA Accession | Condition |
|---|---|---|
| Control Replicate 1 | SRR24401889 | 0 PSU |
| Control Replicate 2 | SRR24401893 | 0 PSU |
| Treatment Replicate 1 | SRR24401886 | 5 PSU, 24h |
| Treatment Replicate 2 | SRR24401892 | 5 PSU, 24h |

Raw sequencing data were retrieved from publicly available NCBI repositories and processed using Galaxy.

---

## 5. Reference Genome and Annotation

**Organism:** *Chara braunii*

**Reference genome accession:** GCA_003427395.1

**Genome assembly:** Cbr_1.0

**Genome source:** NCBI

**Annotation used by the group:** NCBI GTF gene annotation

The reference genome was used as the basis for read alignment and gene-level expression analysis.

---

## 6. Original Authors' RNA-Seq Pipeline

The original study employed the following analytical pipeline:

1. FastQC and MultiQC – assessment of raw sequencing quality.
2. Cutadapt – removal of adapter contamination and trimming of sequencing reads.
3. STAR – alignment of RNA-seq reads to the reference genome.
4. STAR gene counting – quantification of reads associated with gene features.
5. DESeq2 – differential gene expression analysis.
6. goseq – Gene Ontology enrichment analysis.

---

## 7. Galaxy Pipeline Used by the Group

The group performed RNA-seq processing and analysis using Galaxy.

### Workflow

Raw paired-end RNA-seq reads  
↓  
FastQC – Quality assessment  
↓  
Cutadapt – Read trimming  
↓  
STAR – Genome alignment  
↓  
STAR GeneCounts – Gene-level read quantification  
↓  
DESeq2 – Differential gene expression analysis  
↓  
Interpretation of differentially expressed genes

### Software and Tools

| Analysis Step | Tool |
|---|---|
| Quality control | FastQC |
| Adapter trimming | Cutadapt |
| Read alignment | STAR |
| Gene counting | STAR GeneCounts |
| Differential expression | DESeq2 |

---

## 8. Differences Between the Original and Group Pipeline

| Analysis Step | Original Study | Group Re-analysis |
|---|---|---|
| Quality control | FastQC and MultiQC | FastQC |
| Trimming | Cutadapt | Cutadapt |
| Alignment | STAR | STAR |
| Gene counting | STAR using OrcAE GTF | STAR GeneCounts using NCBI GTF |
| Differential expression | DESeq2 | DESeq2 |
| Gene Ontology analysis | goseq | Not included in the current analysis |

The group followed a comparable RNA-seq analysis pipeline but used an NCBI gene annotation file instead of the OrcAE annotation used in the original study. Differences in annotation and data processing may influence gene-level results and the comparison of differentially expressed genes.

---

## 9. Quality Control Results

FastQC was performed on the paired-end sequencing reads to assess their quality before processing.

The quality-control results generally showed acceptable overall sequence quality across the samples. However, some files showed warnings or failures in metrics such as per-base sequence content, sequence duplication levels, overrepresented sequences, and per-sequence GC content.

These observations were considered during the subsequent analysis.

### Quality-Control Evidence

- FastQC reports for all samples
- FastQC screenshots from Galaxy
- Quality-control observations recorded in the report

---

## 10. Read Trimming

Cutadapt was used to remove adapter contamination and prepare the sequencing reads for genome alignment.

The trimming process was performed in Galaxy using paired-end RNA-seq data.

The trimmed reads were subsequently used for STAR alignment.

### Evidence
- Cutadapt output
- Galaxy screenshots
- Trimming history

---

## 11. Genome Mapping

STAR was used to align the trimmed paired-end reads to the *Chara braunii* reference genome.

The reference genome assembly used was GCA_003427395.1 (Cbr_1.0).

Mapping results were assessed through the STAR output generated in Galaxy.

### Evidence
- STAR alignment results
- Mapping statistics
- Galaxy history screenshots

---

## 12. Gene Counting

STAR GeneCounts was used to obtain gene-level read counts from the aligned RNA-seq data.

The resulting gene count data were used as input for differential expression analysis.

### Evidence
- Gene count output
- Galaxy processing history

---

## 13. Differential Gene Expression Analysis

Differential gene expression analysis was performed using DESeq2 to compare the control samples against the salt-treated samples.

### Conditions Compared

- Control: 0 PSU freshwater
- Treatment: 5 PSU salt stress, 24 hours

### Analysis Summary

| Metric | Result |
|---|---|
| Gene features in dataset | 35,420 |
| Valid p-values | 18,764 |
| Adjusted p-values | 12,867 |
| Significant DEGs reported | 174 |
| Positive log2 fold change | 69 |
| Negative log2 fold change | 105 |
| Adjusted p-value threshold | 0.05 |

The group identified 174 differentially expressed gene features based on the reported adjusted p-value threshold of 0.05.

### Evidence
- DESeq2 output
- MA plot
- Galaxy screenshots

---

## 14. Selected Differentially Expressed Genes

Three genes were selected for interpretation based on their differential expression results.

| Gene ID | Log2 Fold Change | Adjusted P-value | Direction |
|---|---:|---:|---|
| CBR_g17758 | 4.6674 | 1.5285e-22 | Upregulated |
| CBR_g25795 | 4.5073 | 1.5285e-22 | Upregulated |
| CBR_g770 | -3.1771 | 4.6744e-14 | Downregulated |

### Interpretation

The selected genes exhibited changes in transcript abundance between freshwater control and salt-treated samples.

Positive log2 fold-change values indicate higher expression in the treatment condition relative to the control, while negative values indicate lower expression in the treatment condition.

Gene interpretation should be based on available gene annotations and published information. The differential expression of a gene alone does not establish its biological function in salt tolerance.

---

## 15. Comparison With the Original Study

The group compared its differential expression findings with those reported by Heß et al. (2023).

Although both analyses investigated the transcriptional response of *Chara braunii* to salinity stress, differences in sample selection, annotation, and analytical processing may contribute to differences in the resulting gene expression patterns.

One observed discrepancy involved CBR_g770, which showed a negative log2 fold-change in the group's analysis despite being reported as increased in the original study.

Such differences highlight the importance of annotation consistency, sample selection, analytical parameters, and reproducibility in RNA-seq studies.

---

## 16. Limitations

The current re-analysis has several limitations:

- Only four RNA-seq samples were included.
- The analysis was limited to two biological replicates per condition.
- The group used a different gene annotation source from the original study.
- The re-analysis did not fully reproduce every analytical step performed by the original authors.
- Gene Ontology enrichment analysis was not included in the current analysis.
- Differences in analytical settings and annotation may affect the comparability of the results.

These limitations should be considered when interpreting the findings.

---

## 17. Conclusion

The RNA-seq re-analysis of *Chara braunii* under salinity stress demonstrated how public sequencing data can be processed and analyzed using Galaxy to investigate changes in gene expression.

The group performed quality control, read trimming, genome alignment, gene counting, and differential gene expression analysis using a Galaxy-based pipeline. The results identified differentially expressed gene features between freshwater control and salt-treated samples.

Despite differences from the original analytical pipeline, the re-analysis provided an opportunity to evaluate RNA-seq reproducibility, understand gene expression analysis, and examine the effects of methodological differences on transcriptomic results.

---

## 18. References

Heß, D., Heise, C. M., Schubert, H., Hess, W. R., & Hagemann, M. (2023). *The impact of salt stress on the physiology and the transcriptome of the model streptophyte green alga Chara braunii*. Physiologia Plantarum, 175(6), e14123.

NCBI Sequence Read Archive (SRA). BioProject PRJNA965918.

NCBI Genome Assembly: GCA_003427395.1.

---

## 19. Repository Structure

```text
RNA-Seq-Guided-Replication/
│
├── README.md
│
├── paper/
│   └── research-paper-link.txt
│
├── methods/
│   └── pipeline-summary.md
│
├── galaxy_workflow/
│   └── workflow-export.ga
│
├── results/
│   ├── qc-summary.md
│   ├── mapping-summary.md
│   ├── gene-count-summary.md
│   └── differential-expression-summary.md
│
├── figures/
│   ├── fastqc/
│   ├── mapping/
│   └── differential-expression/
│
└── student_contributions/
    └── contributions.md
```

*Large FASTQ, BAM, and reference genome files are not uploaded to GitHub. Public accession numbers and repository links are provided instead.*
