# Staphylococcus aureus – NGS pipeline (FastQC → trimming → BWA/SAMtools → variant calling)

Repozytorium zawiera prosty, powtarzalny pipeline do analizy dwóch próbek *Staphylococcus aureus* (SRR30615855 i SRR30615856; projekt PRJNA1158983) – od pobrania danych, przez QC i mapowanie do genomu referencyjnego, po wywołanie wariantów oraz podstawowe raporty.

> Ten repo to „wersja do oddania / pokazania” na GitHub: ma czytelną strukturę, instrukcję uruchomienia i skrypty. Wyniki (duże pliki) są ignorowane przez `.gitignore`.

## Co jest w środku?

- `scripts/pipeline.sh` – główny pipeline end‑to‑end (SRA → FASTQ → QC → trimming → mapowanie → warianty).
- `scripts/map_bwa.sh` – osobny skrypt mapowania (BWA MEM + sort + index + flagstat).
- `environment.yml` – środowisko conda z narzędziami (bioconda).
- `report/` – Twoja prezentacja / opis projektu w PDF.
- `docs/` – krótkie notatki (np. VEP/GO enrichment).

## Wymagania

Najprościej użyć Conda/Mamby:

```bash
# mamba (szybciej) lub conda
mamba env create -f environment.yml
mamba activate staph-ngs
```

Wymagane narzędzia (instalowane z env): `sra-tools`, `fastqc`, `multiqc`, `fastp`, `bwa`, `samtools`, `bcftools`, (opcjonalnie `gatk4`, `vcftools`).

## Szybki start

```bash
chmod +x scripts/*.sh
./scripts/pipeline.sh
```

Pipeline utworzy katalogi:
- `ref/` – referencja (genom) i indeksy,
- `data/` – surowe i przycięte FASTQ,
- `results/` – BAM, VCF, raporty,
- `logs/` – logi z uruchomienia.

## Dane wejściowe (domyślnie)

W `scripts/pipeline.sh` ustawione są:
- `SAMPLE_IDS=(SRR30615855 SRR30615856)`
- referencja: `REF_ACC=GCF_000013425.1` (pobierana jako ZIP z NCBI Datasets API i rozpakowywana do `ref/`)

Jeśli chcesz użyć innych SRR albo innej referencji – edytuj zmienne na początku `scripts/pipeline.sh`.

## Wyniki

Najważniejsze pliki (po udanym przebiegu):
- `results/<SAMPLE>.sorted.bam` + `.bai` + `results/<SAMPLE>.flagstat.txt`
- `results/<SAMPLE>.vcf.gz` (warianty z bcftools) + indeks `.tbi`
- `results/qc_raw/` i `results/qc_trimmed/` (FastQC + MultiQC)
- `results/trim/` (raporty fastp)

## Analizy „poza skryptem” (VEP / GO)

Zobacz: `docs/downstream_vep_go.md`.

## Autor

Kamil Jaworowski
