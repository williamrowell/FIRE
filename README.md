# 🔥 **FIRE**: <ins>F</ins>iber-seq <ins>I</ins>nferred <ins>R</ins>egulatory <ins>E</ins>lements
A pipeline for calling Fiber-seq Inferred Regulatory Elements (FIREs) on single molecules.

## Configuring

See `config/config.yaml` and `config/config.tbl` for configuration options.

## Install
Add a snakemake conda prefix to your `bashrc`, e.g. in the Stergachis lab add:
```bash
export SNAKEMAKE_CONDA_PREFIX=/mmfs1/gscratch/stergachislab/snakemake-conda-envs
```
Then snakemake should install all the additional requirements as a conda env in that directory. 

## Model
Unless directed otherwise it would be best to use this model for your data:
```bash
/mmfs1/gscratch/stergachislab/mvollger/projects/GM12878_aCRE_2022-08-16/results/new_feats_GM12878/model.dat
```

## Run
```bash
snakemake \
  --configfile config/config.yaml \
  --local-cores $(nproc) \
  --cores $(nproc) \
  --rerun-triggers mtime \
  --rerun-incomplete \
  --scheduler greedy \
  --resources load=1000 \
  --show-failed-logs \
  -p 
```
And modify as needed for distributed execution. 

# Outputs
```bash
___ FIRE.bed.gz # Every FIRE element on every fiber
___ FIRE.peaks.with.coverage.bed # Every FIRE peak in the genome, filtered for coverage
___ percent-in-clusters.txt # Percent of FIREs that are in clusters (QC metric)
___ test.peaks-vs-percent.pdf # Plot of of the above
___ clustering-vs-null.bed.gz # A null distribution of FIRE.bed.gz for measuring clustering
___ coverage # Whole genome coverage tracks
_  ___ test.bed.gz # bed graph of coverage
_  ___ test.d4 # bed graph of coverage in d4 format
_  ___ test.median.chromosome.coverage.bed # median coverage per chromosome
_  ___ test.median.coverage.txt # median coverage across the genome
___ all # results for all fibers
_  ___ acc.model.results.bed.gz # All nucleosomes and MSPs with their FIRE scores for all fibers
_  ___ acc.model.results.bed.gz.tbi # index for above
_  ___ peak.calls.bed # FIRE peaks without coverage or significance filtering 
_  ___ fdr.peaks.and.coverages.bed.gz # FIRE peaks with coverage but no significance filtering 
_  ___ fdr.peaks.and.coverages.bed.gz.tbi # index for above
_  ___ chromosomes/ # per chromosome results, can be ignored
___ hap1 # results for hap1 fibers
_  ___ # same structure as the all directory
___ hap2 # results for hap2 fibers
_  ___ # same structure as the all directory
___ unk # results for fibers from an unknown haplotype
_  ___ # same structure as the all directory
___ hap1-vs-hap2 # results for comparing hap1 and hap2 fibers
_  ___ FIRE.hap.differences.bed # Large table of FIREs that are different between hap1 and hap2
_  ___ FIRE.hap.differences.bed9 # Same as above but in bed9 format
_  ___ hap1-vs-hap2-volcano.pdf # Volcano plot of FIREs that are different between hap1 and hap2
_  ___ hap1-vs-hap2.pdf # Scatter plot of FIREs and their percent accessibility for each hap
___ trackHub/ # a trackhub directory that can be loaded into the UCSC browser
```