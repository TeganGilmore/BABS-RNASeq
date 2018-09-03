
## Results and interpretation

The following directories will be created in the output directory after the pipeline has finished.

### `qc/`

#### Read and alignment QC

| Directory          | Description                                                                                                                                                                                                       |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `qc/fastqc/raw/`   | [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) results for read 1 and read2 **before** adapter trimming.                                                                                    |
| `qc/fastqc/trim/`  | [FastQC](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) results for read 1 and read2 **after** adapter trimming.                                                                                     |
| `qc/fastq_screen/` | [FastQ Screen](https://www.bioinformatics.babraham.ac.uk/projects/fastq_screen/) results for read 1 and read2 **before** adapter trimming.                                                                        |
| `qc/cutadapt/`     | Log files generated by [cutadapt](http://cutadapt.readthedocs.io/en/stable/installation.html) containing adapter trimming information.                                                                            |
| `qc/multiqc/`      | HTML file generated by [MultiQC](http://multiqc.info/) to collate pipeline QC from FastQC, FastQ Screen, cutadapt, samtools flagstat, samtools idxstats, picard CollectMultipleMetrics and picard MarkDuplicates. |

### `align/`

#### Run-level results

| Directory               | Description                                                                                                                                                                                                                                 |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `align/`                | Filtered, coordinate sorted alignment files in [BAM](https://samtools.github.io/hts-specs/SAMv1.pdf) format at the run-level for each sample.                                                                                               |
| `align/flagstat/`       | Multiple BAM files will be generated before the final filtered BAM file is created. The [SAMtools flagstat](http://www.htslib.org/doc/samtools.html) files for a selection of these will be placed in this directory.                       |
| `align/idxstats/`       | SAMtools [idxstats](http://www.htslib.org/doc/samtools.html) files to determine the percentage of reads mapping to mitochondrial DNA.                                                                                                       |
| `align/picard_metrics/` | Alignment QC files from picard [CollectMultipleMetrics](https://broadinstitute.github.io/picard/command-line-overview.html) and the metrics file from [MarkDuplicates](https://broadinstitute.github.io/picard/command-line-overview.html). |
| `align/sysout/`         | Sysout files for various programs to aid in troubleshooting errors.                                                                                                                                                                         |

#### Replicate-level results

| Directory                                                    | Description                                                                                                                                                                                       |
|--------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `align/replicateLevel/`                                      | Replicate-level, merged, coordinate sorted BAM files after the re-marking and removal of duplicates.                                                                                              |
| `align/replicateLevel/flagstat/`                             | Flagstat files associated with the final filtered merged BAM file.                                                                                                                                |
| `align/replicateLevel/picard_metrics/`                       | Metrics file from MarkDuplicates.                                                                                                                                                                 |
| `align/replicateLevel/sysout/`                               | Sysout files for various programs to aid in troubleshooting errors.                                                                                                                               |
| `align/replicateLevel/bigwig/`                               | Normalised [bigWig](https://genome.ucsc.edu/goldenpath/help/bigWig.html) files scaled to 1 million mapped read pairs.                                                                             |
| `align/replicateLevel/macs2/`                                | Peak sets per sample called in "broadPeak" mode with [MACS2](https://github.com/taoliu/MACS).                                                                                                     |
| `align/replicateLevel/macs2/qc/`                             | Various QC plots per sample including number of peaks, fold-change distribution, [FRiP score](https://genome.cshlp.org/content/22/9/1813.full.pdf+html) and peak percentage across gene features. |
| `align/replicateLevel/macs2/merged_peaks/`                   | Merged peakset across all samples.                                                                                                                                                                |
| `align/replicateLevel/macs2/merged_peaks/deseq2/`            | Differential binding analysis, PCA and clustering using reads quantified at the replicate-level for the merged peak set.                                                                          |

#### Sample-level results

`align/replicateLevel/` and `align/sampleLevel/` have exactly the same directory structure. The only difference is that multiple libraries sequenced from the same sample will be merged in the `replicateLevel/` directory whereas all the replicates associated with a sample will be merged in the `sampleLevel/` directory. The main reasoning behind merging the replicates at the sample-level was to improve the coverage in order to call more accurately defined and robust peaksets. This is also useful for downstream analyses such as [motif footprinting](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3959825/) which requires high coverage.

### `genome/`

Genome-specific files required by selected processes in the pipeline.

### `igv/`

An [IGV](https://software.broadinstitute.org/software/igv/) session file called `igv_session.xml` will be created at the end of the pipeline. This avoids having to load all the data individually into IGV for visualisation. Once installed, open IGV, go to `File > Open Session` and select the `igv_session.xml` file for loading.

File paths in the IGV session file will be set as absolute paths to the directory containing the results. If you prefer to load the data over the web you can just replace the relevant portion of the file path with a link in the session file.

The reference genome fasta file will be soft-linked to the `genome/` directory, and by default this will be set as the genome for the IGV session. If you prefer to use an in-built genome provided by IGV just change the file path to the name of the IGV genome e.g. mm10 or hg19.   

### `pipeline/`

Nextflow provides excellent functionality for generating various reports relevant to the running and execution of the pipeline. This will allow you to trouble-shoot errors with the running of the pipeline, and also provide you with other information such as launch commands, run times and resource usage. Default reports generated by the pipeline are `BABS-ATACSeqPE_report.html`, `BABS-ATACSeqPE_timeline.html`, `BABS-ATACSeqPE_trace.txt` and `BABS-ATACSeqPE_dag.dot`.

See [Nextflow Tracing & visualisation](https://www.nextflow.io/docs/latest/tracing.html)