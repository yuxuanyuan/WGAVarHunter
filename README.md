# WGAVarHunter
Fast and accurate genetic variation identification through whole genome alignment

## Welcome to Whole Genome Alignment-based Variation Hunter (WGAVarHunter)

Basically, the program was written in [Rust](https://www.rust-lang.org/) and it needs [samtools](http://www.htslib.org/download/), sequence aligner [minimap2](https://github.com/lh3/minimap2), [winnowmap](https://github.com/marbl/Winnowmap), [unimap](https://github.com/lh3/unimap) or [wfmash](https://github.com/waveygang/wfmash). Users need to install the required tools before using ``WGAVHunter``.

### Pre-compiled version

Here I have attached a ``pre-compiled`` edition under ``Linux`` and ``macOS`` environments. 

1. In ``Linux`` environment, users may need to check ``glibc`` by using ``ldd --version``. If the version is ``2.17`` or ``above``, you can freely use the binary tool.

2. In ``macOS`` environment, it was compiled in ``macOS Monterey``.

### Execution

You can check ``help`` by using ``WGAVHunter -h``

```
----------------------------------------------------------------------------------------------------
Program: WGAVHunter
Version: 0.1.0
Author:  Andy Yuan (yuxuan.yuan@outlook.com)
----------------------------------------------------------------------------------------------------
Synopsis: Discover genomic variants based on whole genome alignment through an efficient way

USAGE:
    WGAVHunter [OPTIONS] -r <REFERENCE> -q <QUERY>... -o <OUTDIR>

OPTIONS:
    -r <REFERENCE>                 A reference fasta file
    -q <QUERY>...                  Query fasta file(s). Can be single or multiple
    -n <N_PLOIDY>                  Ploidy level of the species [default: 2]
    -w <WINDOW_SIZE>...            Window size(s) used to split the query fasta (kb). Can be single
                                   or multiple values [default: 500]
    -P <PERCENTAGE>                Percentage (%) of adjacent windows overlapped [default: 10]
    -a <ALIGNER>                   Aligner (minimap2|winnowmap|unimap|wfmash) [default: minimap2]
    -A <ALIGNER_SETTINGS>          Aligner parameter settings in "" [default: "-x asm20"]
    -u <USE_SPLIT>                 Use 'split-prefix' for (minimap2|winnowmap). Could be pretty slow
                                   and storage demanding if the genome size is big [default: false]
    -c <CHUNK_SIZE>                Chunk size (kb) used to parse each chromosome [default: 1000]
    -R <REMOVE_UNQUALIFIED>        Remove query seq with less than n (bp) aligned [default: 1000]
    -m <MAP_QUALITY>               Mapping quality used for variant calling [default: 30]
    -s <CALL_SNVS>                 Call single nucleotide variants (SNVs) [default: true]
    -I <CALL_SMALL_INDELS>         Call small indels [default: true]
    -S <CALL_SVS>                  Call structural variants (SVs) [default: true]
    -N <NOVEL_REGIONS>             Report novel genomic regions in the input fastas [default: true]
    -M <MAX_INDEL_SIZE>            Maximum small indel size (bp) called [default: 49]
    -d <DEFAULT_SV_SIZE>           Minimum SV size (bp) called [default: 50]
    -D <DIST_DUP>                  Maximum allowed distance (bp) between aligned query coordinates
                                   for duplication calling [default: 1000]
    -t <THREADS>                   Number of threads [default: 4]
    -p <PREFIX>                    Prefix of the output files [default: WGAVHunter]
    -o <OUTDIR>                    Output directory
    -i <INTER_DIR>                 Intermediate folder [default: $OUTDIR/tmp]
    -k <KEEP_INTER>                Keep intermediate folder and content [default: false]
    -e <ENABLE_DEBUG>              Enable debug mode [default: false]
    -h, --help                     Print help information
    -V, --version                  Print version information
```

And then you may run the program like

```
./WGAVHunter -r ref.fa -q qry.fa -o .
```

Parameter settings can be adjusted based on the resources.


**TODO**

- [ ] Translocation callng is under development


**Notification**

- [ ] Variations called from duplication regions or (duplication + inversion regions) may be incorrect. Users may need to further filter. 
- [ ] Due to the limitation of BAM format, the window size cannot be over 256 Mb
- [ ] For large genome comparison (for example genomes > 4Gb), we recommend using ``unimap`` or ``wfmash`` for alignment. 
