# Variant calls on ancient genomes: linear reference vs pangenome graph

Exploratory analysis from my PhD at the Australian Centre for Ancient DNA (University of Adelaide). It asks how the choice of mapping and variant-calling pipeline changes the variants recovered from ancient human genomes, and what those variants are predicted to do.

## Question

Ancient DNA is short, damaged and usually low coverage. Mapping it to a linear reference genome biases calls towards the reference allele. Pangenome graphs built with [vg](https://github.com/vgteam/vg) can reduce that bias. This project compares, for the same samples:

| Pipeline | Mapping | Variant calling |
|---|---|---|
| `BWA` | BWA on the linear reference | FreeBayes |
| `VGbam` | vg graph mapping following Martiniano et al. (2020), output as BAM | FreeBayes |
| `VGgam` | vg graph mapping following Martiniano et al. (2020), output as GAM | vg call |

Every VCF was annotated with Ensembl VEP: variant class, IMPACT, SIFT, PolyPhen and GERP.

## Samples

| Sample | Approx. age (years BP) |
|---|---|
| BR2 | 2,800 |
| BOT2016 | 5,400 |
| SF12 | 8,895 |
| Ust'Ishim | 44,000 |

SF12 was also downsampled to several coverages to separate the effect of coverage from the effect of sample age.

## Notebooks

| Notebook | What it does |
|---|---|
| `VEP_analysis.ipynb` | Loads the VEP output per sample and pipeline, documents the VEP fields, compares IMPACT, SIFT and PolyPhen between BWA, VGbam and VGgam |
| `Create_big_file.ipynb` | Summarises every annotated VCF into one table (counts and proportions per variant class and per consequence), written to `Final_DF.csv` |
| `Overall_plots.ipynb`, `Overall_plots2.ipynb` | Interactive Plotly figures across samples: number and type of variants, IMPACT, SIFT, PolyPhen and GERP, by sample age and by coverage |
| `VCF_EDA_normalized_vs_original*.ipynb` | Checks whether the differences between pipelines come from VCF normalisation. vg call normalises its output and FreeBayes does not, so the VCFs are compared before and after normalisation |

## Notes

- The raw BAM/GAM/VCF files are not in the repository (too large). The notebooks read VEP text files from a local `SmallFiles/` folder. `Final_DF.csv` holds the summary table, so the plotting notebooks run from it directly.
- The plots are interactive (Plotly). GitHub doesn't render them, so open the notebooks in [nbviewer](https://nbviewer.org/github/AdrienOliva/Indel_Project/tree/main/) or run them locally.
- Requires Python 3 with `pandas`, `plotly` and `requests`.

Related published work: [Systematic benchmark of ancient DNA read mapping](https://doi.org/10.1093/bib/bbab076) (Briefings in Bioinformatics, 2021) and [code](https://github.com/AdrienOliva/Benchmark-aDNA-Mapping).
