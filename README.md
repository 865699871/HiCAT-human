# HiCAT-human

## Dependencies

Python 3.9.13

| Packages           | Version |
| ------------------ | ------- |
| biopython          | 1.79    |
| joblib             | 1.1.0   |
| lastz              | 1.04.22 |
| matplotlib         | 3.5.1   |
| numpy              | 1.22.3  |
| pandas             | 1.4.0   |
| python-edlib       | 1.3.9   |
| python-levenshtein | 0.12.2  |
| scikit-learn       | 1.0.2   |
| setuptools         | 61.2.0  |

StringDecomposer (https://github.com/ablab/stringdecomposer)  version 1.1.2.
Development environment: Linux
Development tool: Pycharm

## Installation

#### Source code (g++ version 5.3.1 or higher for stringdecomposer)

```
#install
conda install -y --file requirements.txt
cd ./stringdecomposer && make
```

## Usage

HiCAT-human includes six modes.

#### Reads annotation

```
HiCAT_human.py reads [-h] -r INPUT_READS_DIR -rs READS_SAMPLE_FILE -o READS_OUTPUT_DIR [-th THREAD]

optional arguments:
  -h, --help            show this help message and exit
  -r INPUT_READS_DIR, --input_reads_dir INPUT_READS_DIR
                        Input reads directory containing all sample read file (.fasta) and corresponding .fai file, required
  -rs READS_SAMPLE_FILE, --reads_sample_file READS_SAMPLE_FILE
                        File record all sample names (the prefix of .fasta file), one sample name per line, required
  -o READS_OUTPUT_DIR, --reads_output_dir READS_OUTPUT_DIR
                        HiCAT human reads output path, required
  -th THREAD, --thread THREAD
                        The number of threads, default is 1
```

`input_reads_dir` should contain the fasta format read file of all samples and corresponding .fai file (like sample1.fasta, sample1.fasta.fai, sample2.fasta, sample2.fasta.fai ...). `reads_sample_file` should be a text file record one sample name per line. 

For each sample, following output files (directories)  are generated:

```
chr_data
hicat_reads
merge.fa
merge.label.xls
SD
```

`chr_data `contains the fasta format file of reads classified to each chromosome and the  stringdecomposer result of them.

`hicat_reads`contains the annotation results of reads on each chromosome.

```
+ out_all_layer.xls: the annotation in all layer. Label "top" represent this region is in top layer. Label "cover" represent this region is covered by a top layer region.
(read name, start in block sequence, end in block sequence, repeat number, pattern in monomer sequence format, HOR name, type)
SRR11292120.222 69      18435   9       3_1_2_5_6_4     R2L6    top
SRR11292120.222 239     1599    4       1_2     R1L2    cover
SRR11292120.222 2278    3637    4       1_2     R1L2    cover
+ out_top_layer.xls: the annotation in top layer. 
(read name, start in block sequence, end in block sequence, repeat number, pattern in monomer sequence format, HOR name)
SRR11292120.222 69      18435   9       3_1_2_5_6_4     R2L6
SRR11292120.222 18605   19624   3       1_2     R1L2
+ monseq.xls: monomer sequence of reads. 
+ out_final_hor.xls: HOR patterns.
+ out_hor.raw.fa: HOR DNA sequences. Each sequence named as HORname::readname-start-end::strand.
+ out_hor.normal.fa: Normalized HOR DNA sequence. We normalized the raw DNA sequence to one represent HOR. For example, 10_9_8_4_(7_6_5_4_7_6_5_4)_3_2_1 to 1_2_3_4_5_6_7_4_8_9_10 in CEN21.
```

`merge.label.xls` contains all alpha satellite reads and the chromosomes they are classified to. `merge.fa` contains the sequence of these reads.

`SD` contains the stringdecomposer result.

#### Assembly annotation

```
HiCAT_human.py assembly [-h] -a INPUT_ASSEMBLY_DIR -as ASSEMBLY_SAMPLE_FILE [-ms MIN_SIMILARITY] [-mh MAX_HOR_LEN] [-sp SHOW_HOR_NUMBER] [-sn SHOW_HOR_MIN_REPEAT_NUMBER] [-th THREAD]

optional arguments:
  -h, --help            show this help message and exit
  -a INPUT_ASSEMBLY_DIR, --input_assembly_dir INPUT_ASSEMBLY_DIR
                        Input assembly directory containing all sample assembly file (.fasta) corresponding .fai file, required
  -as ASSEMBLY_SAMPLE_FILE, --assembly_sample_file ASSEMBLY_SAMPLE_FILE
                        File record all sample names (the prefix of .fasta file), one sample name per line, required
  -ms MIN_SIMILARITY, --min_similarity MIN_SIMILARITY
                        Lower bound for similarity threshold between block and target monomer, default is 0.9
  -mh MAX_HOR_LEN, --max_hor_len MAX_HOR_LEN
                        Upper bound of the tandem repeat unit length for improving efficiency, default 40 monomers
  -sp SHOW_HOR_NUMBER, --show_hor_number SHOW_HOR_NUMBER
                        Visualized HOR number, default 5
  -sn SHOW_HOR_MIN_REPEAT_NUMBER, --show_hor_min_repeat_number SHOW_HOR_MIN_REPEAT_NUMBER
                        HORs with repeat number less than this threshold will not be visualized, default 10
  -th THREAD, --thread THREAD
                        The number of threads, default is 1
```

`input_reads_dir` should contain the fasta format genome of all samples and corresponding .fai file (like sample1.fasta, sample1.fasta.fai, sample2.fasta, sample2.fasta.fai ...). `assembly_sample_file` should be a text file record one sample name per line. 

For each chromosome of each sample, chromosome-specific alpha satellite sequences and the lastz result are generated. The annotation results of each chromosome are in `censeq` directory.

```
+ 1.fa, 2.fa: ?
+ chr.cen.fa (input_fasta.1.fa): chromosome-specific alpha satellite sequence
+ chr.region.xls:the continuous regions in chr.cen.fa
+ final_decomposition_raw.tsv: ?
+ final_decomposition.tsv: the result of stringdecomposer.
+ hor.repeatnumber.xls: the repeat number of HORs.
+ out_block.sequences: block sequence.
+ out_all_layer.xls: the annotation in all layer. Label "top" represent this region is in top layer. Label "cover" represent this region is covered by a top layer region.
+ out_top_layer.xls: the annotation in top layer. 
+ out_monomer_seq.xls: monomer sequence of chromosome-specific alpha satellite sequence
+ out_monomer.fa: sequence and location of each monomer
+ out_final_hor.xls: HOR patterns.
+ out_cluster_X.xls: 不denovo聚类也有cluster文件吗？
+ out_hor.raw.fa: HOR DNA sequences. Each sequence named as HORname::start-end::strand.
+ out_hor.normal.fa: Normalized HOR DNA sequence.
+ pattern_static.pdf: Bar plot of HOR repeat number.
+ pattern_static.xls: HOR repeat number.
+ plot_pattern.pdf: the location distribution of HOR annotation.
+ re_eddis.xls: ?
```

#### Reads aggregate

```
HiCAT_human.py reads_aggregate [-h] -i ALL_SAMPLE_DIR -s SAMPLE_FILE [-ref REF_GENOME_SIZE] [-rr RARE_RATIO] [-u PLOT_UPPER]

optional arguments:
  -h, --help            show this help message and exit
  -i ALL_SAMPLE_DIR, --all_sample_dir ALL_SAMPLE_DIR
                        All sample HiCAT human reads result path, required
  -s SAMPLE_FILE, --sample_file SAMPLE_FILE
                        Two-column file record all sample names and gender, \t separator, required
  -ref REF_GENOME_SIZE, --ref_genome_size REF_GENOME_SIZE
                        Reference human genome size (base number), default is the base number of CHM13
  -rr RARE_RATIO, --rare_ratio RARE_RATIO
                        Exclude HORs with less frequency than this ratio in all samples, default is 0.1
  -u PLOT_UPPER, --plot_upper PLOT_UPPER
                        Mean fold-change upper bound in the output plot, default is 5
```

The number of HOR patterns in all samples are calculated. Results are in `pattern.summary.xls`, `pattern.table.xls` and `all.sample.HOR.repeatnumber.matrix.xls`. HOR patterns are renamed based on this result and reads annotation results are revised into files with `merge` in their names.

Gender information of samples are used to calculate the normalized numbers (n-number) of HORs. `HOR_variation.pdf` showed the variation of HOR n-number mean fold-change among samples.

#### Assembly match to reads

```
HiCAT_human.py assembly_match [-h] -p PATTERN_FILE -s SAMPLE_FILE -a HICAT_HUMAN_ASSEMBLY_DIR [-sp SHOW_HOR_NUMBER] [-sn SHOW_HOR_MIN_REPEAT_NUMBER]

optional arguments:
  -h, --help            show this help message and exit
  -p PATTERN_FILE, --pattern_file PATTERN_FILE
                        pattern.table.xls, output file of reads_aggregate pipeline, record all patterns in HiCAT human reads result, required
  -s SAMPLE_FILE, --sample_file SAMPLE_FILE
                        File record all sample names, required
  -a HICAT_HUMAN_ASSEMBLY_DIR, --hicat_human_assembly_dir HICAT_HUMAN_ASSEMBLY_DIR
                        HiCAT human assembly result path
  -sp SHOW_HOR_NUMBER, --show_hor_number SHOW_HOR_NUMBER
                        Visualized HOR number, default 5
  -sn SHOW_HOR_MIN_REPEAT_NUMBER, --show_hor_min_repeat_number SHOW_HOR_MIN_REPEAT_NUMBER
                        HORs with repeat number less than this threshold will not be visualized, default 10
```

The assembly annotation results are revised into files with `merge` in their names based on the HOR pattern names of reads annotation result.

#### Only reads

```
HiCAT_human.py only_reads [-h] -r INPUT_READS_DIR -rs READS_SAMPLE_FILE -o READS_OUTPUT_DIR [-th THREAD] [-ref REF_GENOME_SIZE] [-rr RARE_RATIO] [-u PLOT_UPPER]

optional arguments:
  -h, --help            show this help message and exit
  -r INPUT_READS_DIR, --input_reads_dir INPUT_READS_DIR
                        Input reads directory containing all sample read file (.fasta) and corresponding .fai file, required
  -rs READS_SAMPLE_FILE, --reads_sample_file READS_SAMPLE_FILE
                        File record all sample names (the prefix of .fasta file), one sample name per line, required
  -o READS_OUTPUT_DIR, --reads_output_dir READS_OUTPUT_DIR
                        HiCAT human reads output path, required
  -th THREAD, --thread THREAD
                        The number of threads, default is 1
  -ref REF_GENOME_SIZE, --ref_genome_size REF_GENOME_SIZE
                        Reference human genome size (base number), default is the base number of CHM13
  -rr RARE_RATIO, --rare_ratio RARE_RATIO
                        Exclude HORs with less frequency than this ratio in each sample, default is 0.1
  -u PLOT_UPPER, --plot_upper PLOT_UPPER
                        Mean fold-change upper bound in the output plot, default is 5
```

A combination of reads annotation and aggregate.

#### Reads with assembly

```
HiCAT_human.py reads_with_assembly [-h] -r INPUT_READS_DIR -rs READS_SAMPLE_FILE -o READS_OUTPUT_DIR [-th THREAD] -a INPUT_ASSEMBLY_DIR -as ASSEMBLY_SAMPLE_FILE [-ms MIN_SIMILARITY] [-mh MAX_HOR_LEN]
                                          [-sp SHOW_HOR_NUMBER] [-sn SHOW_HOR_MIN_REPEAT_NUMBER] [-ref REF_GENOME_SIZE] [-rr RARE_RATIO] [-u PLOT_UPPER]

optional arguments:
  -h, --help            show this help message and exit
  -r INPUT_READS_DIR, --input_reads_dir INPUT_READS_DIR
                        Input reads directory containing all sample read file (.fasta) and corresponding .fai file, required
  -rs READS_SAMPLE_FILE, --reads_sample_file READS_SAMPLE_FILE
                        File record all sample names (the prefix of .fasta file), one sample name per line, required
  -o READS_OUTPUT_DIR, --reads_output_dir READS_OUTPUT_DIR
                        HiCAT human reads output path, required
  -th THREAD, --thread THREAD
                        The number of threads, default is 1
  -a INPUT_ASSEMBLY_DIR, --input_assembly_dir INPUT_ASSEMBLY_DIR
                        Input assembly directory containing all sample assembly file (.fasta) corresponding .fai file, required
  -as ASSEMBLY_SAMPLE_FILE, --assembly_sample_file ASSEMBLY_SAMPLE_FILE
                        File record all sample names (the prefix of .fasta file), one sample name per line, required
  -ms MIN_SIMILARITY, --min_similarity MIN_SIMILARITY
                        Lower bound for similarity threshold between block and target monomer, default is 0.9
  -mh MAX_HOR_LEN, --max_hor_len MAX_HOR_LEN
                        Upper bound of the tandem repeat unit length for improving efficiency, default 40 monomers
  -sp SHOW_HOR_NUMBER, --show_hor_number SHOW_HOR_NUMBER
                        Visualized HOR number, default 5
  -sn SHOW_HOR_MIN_REPEAT_NUMBER, --show_hor_min_repeat_number SHOW_HOR_MIN_REPEAT_NUMBER
                        HORs with repeat number less than this threshold will not be visualized, default 10
  -ref REF_GENOME_SIZE, --ref_genome_size REF_GENOME_SIZE
                        Reference human genome size (base number), default is the base number of CHM13
  -rr RARE_RATIO, --rare_ratio RARE_RATIO
                        Exclude HORs with less frequency than this ratio in each sample, default is 0.1
  -u PLOT_UPPER, --plot_upper PLOT_UPPER
                        Mean fold-change upper bound in the output plot, default is 5
```

A combination of reads annotation，reads aggregate, assembly annotation and assembly matching to reads.

## Contact

If you have any questions, please feel free to contact: [gaoxian15002970749@163.com](mailto:gaoxian15002970749@163.com)
