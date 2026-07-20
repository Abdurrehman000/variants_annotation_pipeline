# Rare Disease VCF Annotation Assignment

## Overview

This assignment demonstrates a **rare-disease variant annotation workflow** using synthetic GRCh38 genomic data. It processes small variants (SNVs/indels) and copy-number variants (CNVs) through separate annotation branches.

## Included Files

- `sample.small_variants.vcf` — unannotated SNV/indel input.
- `sample.cnvs.bed` — unannotated CNV input in four-column BED format.
- `rare_disease_vcf_annotation_pipeline.sh` — main annotation pipeline.
- `annotation_resources.env.example` — resource-path configuration template.
- `sample_annotation_inputs_README.txt` — detailed input instructions.
- `rare_disease_vcf_annotation_notes.txt` — additional pipeline notes.
- `SAMPLE_RARE_001.pipeline.log` — example execution log.

## Main Tools

The workflow supports:

- **Small variants:** bcftools, VEP, SnpEff, ClinVar, gnomAD, ClinGen, SpliceAI, ANNOVAR and InterVar.
- **CNVs:** AnnotSV, ClassifyCNV, ISV-CNV and an optional Horizon command.

## Basic Usage

First create and edit the resource configuration:

```bash
cp annotation_resources.env.example annotation_resources.env
nano annotation_resources.env
```

Then run:

```bash
bash rare_disease_vcf_annotation_pipeline.sh \
  -i sample.small_variants.vcf \
  -n sample.cnvs.bed \
  -o sample_annotation_results \
  -c annotation_resources.env \
  -s SAMPLE_RARE_001 \
  -a GRCh38 \
  -t 8
```

## Output

Results are organized into:

- `snv/` — annotated small-variant files.
- `cnv/` — annotated and classified CNV files.
- `acmg/` — ANNOVAR and InterVar outputs.
- `logs/` — pipeline execution logs.
- `reports/` — output summaries.

## Important Note

The supplied variants are **synthetic teaching data** and must not be used for real clinical diagnosis. All genome references and annotation databases must match **GRCh38/hg38** and use consistent chromosome naming.
