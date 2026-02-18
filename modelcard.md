# Model Card, Genomic Medicine Analytics Stack
## Model summary
This repository provides an end-to-end computational genomics workflow implemented in ` Genomic_Medicine.ipynb`. The notebook contains multiple models (statistical and ML), including:
- GWAS association testing (PC-adjusted, approximate score-test style)
- PRS construction + logistic regression risk modelling
- NMF for mutational signature extraction (96-channel)
- Gaussian mixture models for VAF mixture inference (clonality proxy)
- Logistic regression classifier for AI-assisted variant prioritisation, with interpretability and auditing

The primary deliverable is not a single deployed model, but a **governance-ready analytic stack** that produces reporting-grade outputs.

## Intended use
### Intended use cases
- Teaching and training in genomic medicine analytics (undergraduate and postgraduate)
- Institute-level capability building for precision medicine
- Prototyping governance patterns (calibration, subgroup audits, interpretability) before real clinical deployment
- Demonstration exercise in population genetics, cancer genomics, and trustworthy AI evaluation

### Out-of-scope use cases
- Direct clinical decision-making on patients
- Use on identifiable human genomic data without ethics approval, consent, and institutional governance
- Any claim of clinical validity without real-world validation on external cohorts

## Data
### Data used for development and evaluation
- **Simulated genotype cohort** (individuals SNPs) with LD blocks, ancestry allele-frequency shifts, and missingness patterns
- **Simulated binary trait** with polygenic + confounding components
- **Simulated tumour mutation catalogues** over 96 channels with tumour-specific exposures
- **Simulated VAF distributions** with 1-3 clonal components
- **Simulated annotation-like variant feature table** (CADD-like, constraint, splicing, MAF, VAF context, etc.)

All datasets are generated within the notebook with fixed random seeds for reproducibility.

### Why simulation is used
Simulation enables transparent demonstration of workflow mechanics, diagnostics, and governance patterns without human subjects data.

## Evaluation
### Metrics reported (by module)
- GWAS: Manhattan and QQ plots, ?GC inflation proxy, top hits table
- PRS/risk: AUROC, PR-AUC, Brier score, calibration curves, ancestry-stratified performance
- NMF signatures: reconstruction RMSE vs k, cosine similarity (recovered vs true signatures), exposure heatmaps
- Clonality proxy: inferred number of mixture components (BIC-selected) and fitted densities
- Variant prioritisation: AUROC, PR-AUC, Brier, ROC/PR/calibration plots, permutation importance, partial dependence, ancestry audit

### Key safety-relevant evaluations
- Calibration, because miscalibration can cause clinically harmful probability misinterpretation
- Group-wise auditing, because pooled metrics can mask performance failures in subgroups
- Interpretability, because genomic medicine requires explainable evidence trails for decisions

## Ethical considerations
- No human data are used.
- The notebook demonstrates governance patterns that are essential when transitioning to real cohorts (consent, privacy, equity, documentation, monitoring).

## Limitations
- Simulated data do not capture full complexity of sequencing artefacts, contamination, relatedness, or platform effects.
- GWAS is simplified (not mixed-model, no imputation uncertainty modelling).
- PRS is simplified (no LD-aware shrinkage methods, no ancestry-specific LD modelling).
- Tumour module focuses on SNV 96-channel signatures, not indels/SVs/CNAs or purity-ploidy modelling.
- Clonality is a proxy (VAF mixture modelling) and is not a full phylogenetic / CNA-aware subclonal reconstruction.
- Variant prioritisation is demonstrated with a baseline linear classifier on synthetic annotation-like features and labels.

## Reproducibility
- Fixed random seeds
- Single-notebook execution produces all figures and tables
- Dependencies: NumPy, pandas, scikit-learn, Matplotlib, SciPy (as used in the notebook)

## Key references (PubMed-indexed)
Alexandrov, L.B., Nik-Zainal, S., Wedge, D.C., et al. (2013). *Nature, 500*, 415-421. https://doi.org/10.1038/nature12477  
Alexandrov, L.B., Kim, J., Haradhvala, N.J., et al. (2020). *Nature, 578*, 94-101. https://doi.org/10.1038/s41586-020-1943-3  
Price, A.L., Patterson, N.J., Plenge, R.M., et al. (2006). *Nature Genetics, 38*, 904-909. https://doi.org/10.1038/ng1847  
Richards, S., Aziz, N., Bale, S., et al. (2015). *Genetics in Medicine, 17*, 405-424. https://doi.org/10.1038/gim.2015.30  
Sirugo, G., Williams, S.M., Tishkoff, S.A. (2019). *Cell, 177*, 26-31. https://doi.org/10.1016/j.cell.2019.02.048  
Popejoy, A.B., Fullerton, S.M. (2016). *Nature, 538*, 161-164. https://doi.org/10.1038/538161a
