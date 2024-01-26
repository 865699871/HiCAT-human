# HiCAT-human

We proposed a modified version of our previous HOR annotation tool HiCAT (https://github.com/xjtu-omics/HiCAT) for automatically annotating centromere HOR patterns from both HiFi reads and assemblies of multiple human samples.

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
| seqtk              | 1.2     |
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

## Quick start

#### Only reads (A combination of reads annotation and aggregation)

```
python HiCAT_human.py only_reads -r INPUT_READS_DIR -rs READS_SAMPLE_FILE -o READS_OUTPUT_DIR -th THREAD
```

For more details, please use `-h` .

- `input_reads_dir` should contain the fasta format read file of all samples and corresponding .fai file. 

  ```
  sample1.fasta
  sample1.fasta.fai
  sample2.fasta
  sample2.fasta.fai
  ```

- `reads_sample_file` should be a two-column file record all sample names and gender, \t separator. 

  ```
  sample1	male
  sample2	female
  ```

- `reads_output_dir`is the output directory.

- `th` is number of threads.

#### Reads with assembly (a combination of reads annotation，reads aggregation, assembly annotation and assembly matching to reads)

```
HiCAT_human.py reads_with_assembly -r INPUT_READS_DIR -rs READS_SAMPLE_FILE -o READS_OUTPUT_DIR -th THREAD -a INPUT_ASSEMBLY_DIR -as ASSEMBLY_SAMPLE_FILE
```

For more details, please use `-h` .

- `input_reads_dir` should contain the fasta format read file of all samples and corresponding .fai file. 

- `reads_sample_file` should be a two-column file record all sample names and gender, \t separator. 

- `reads_output_dir`is the output directory.

- `th` is number of threads.

- `input_assembly_dir` should contain the fasta format assembly file of all samples and corresponding .fai file.

  ```
  assembly1.fasta
  assembly1.fasta.fai
  assembly2.fasta
  assembly2.fasta.fai
  ```

  The name of chromosomes in assembly file should start with 'chr'.

  ```
  >chr1
  ACGTACGTACGTCAGATCTACGCATAGTGTGCTA...
  >chr2
  CACAGTGGTGGTGTGGGTTACTACACA...
  ```

- `assembly_sample_file` should be a text file record one sample name per line.

  ```
  assembly1
  assembly2
  ```

## Contact

If you have any questions, please feel free to contact: [gaoxian15002970749@163.com](mailto:gaoxian15002970749@163.com), [xfyang@xjtu.edu.cn](mailto:xfyang@xjtu.edu.cn), [kaiye@xjtu.edu.cn](mailto:kaiye@xjtu.edu.cn)

## Reference

Please cite the following paper when you use HiCAT-human in your work