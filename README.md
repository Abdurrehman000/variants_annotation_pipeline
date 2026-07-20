# BIOMISA Rare-Disease Variant Annotation Assignment

## Project Overview

This assignment develops a reproducible **GRCh38 rare-disease variant annotation workflow** for both small variants and copy-number variants. Synthetic VCF and BED files are used for learning, testing and comparing manual interpretation with automated pipeline results.

## Diseases Studied

| Disease | Main gene(s) | Chromosome/region | Inheritance |
|---|---|---|---|
| Fabry disease | `GLA` | Xq22.1 | X-linked |
| Monilethrix | `KRT81`, `KRT83`, `KRT86` | 12q13.13 | Usually autosomal dominant |
| Androgen Insensitivity Syndrome (AIS) | `AR` | Xq12 | X-linked |
| Neurofibromatosis Type 1 (NF1) | `NF1` | 17q11.2 | Autosomal dominant |

Each disease is analysed using two separate inputs:

- **Small-variant VCF:** SNVs and small insertions/deletions.
- **CNV BED/VCF:** deletions and duplications.

The consistent pipeline names are `fabry`, `monilethrix`, `ais` and `nf1`.

## Workflow

### Small variants

1. Normalize variants with **bcftools**.
2. Add gene, transcript and consequence annotations with **VEP**.
3. Add the `ANN` consequence field with **SnpEff**.
4. Add clinical evidence from **ClinVar**.
5. obtain population-frequency information from the existing **VEP/gnomAD cache**.
6. Add dosage and gene-region evidence from **ClinGen**.
7. Predict splice effects with **SpliceAI**.
8. Produce the final compressed and indexed annotated VCF.

### Copy-number variants

1. Convert CNV calls into four-column BED format: `chrom start end DEL|DUP`.
2. Annotate genes and genomic regions with **AnnotSV**.
3. Apply ACMG-style CNV scoring with **ClassifyCNV**.
4. Generate machine-learning pathogenicity predictions with **ISV-CNV**.
5. Combine the main CNV results into a final summary file.

Manual or optional interpretation may also use InterVar web, OMIM/G2P, AlphaMissense, REVEL and the latest gnomAD website records.

## Main Project Structure

```text
rare_disease_project/
├── containers/          # Apptainer tool containers
├── input/
│   ├── snv/             # Disease small-variant VCF files
│   └── cnv/             # Disease CNV BED/VCF files
├── pipeline/            # Automated annotation script
├── resources/           # GRCh38 reference and annotation databases
└── results/
    ├── fabry/
    ├── monilethrix/
    ├── ais/
    └── nf1/
```

## Running a Disease

```bash
cd ~/rare_disease_project
THREADS=4 bash pipeline/run_rare_disease_pipeline.sh fabry
```

Replace `fabry` with `monilethrix`, `ais` or `nf1` to run another disease.

## Expected Results

Each disease result folder contains:

- final annotated small-variant VCF and Tabix index;
- VEP, SnpEff, ClinVar, ClinGen and SpliceAI annotations;
- AnnotSV output;
- ClassifyCNV results;
- ISV-CNV results;
- combined CNV summary;
- pipeline logs and intermediate working files.

The assignment compares three versions for each disease:

1. unannotated input;
2. manually/GPT-annotated version;
3. pipeline-annotated output.

## Important Notes

- All inputs and resources must use **GRCh38/hg38**.
- Chromosome naming must remain consistent and use the `chr` prefix.
- The full gnomAD dataset is not required; cached VEP frequencies are used and the principal disease variants are checked manually on the gnomAD website.
- The data are synthetic and intended only for education and pipeline testing.
- Automated classifications must not be treated as clinical diagnoses without expert review.
