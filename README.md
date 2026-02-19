# MyGenome
CS485G: Applied Bioinformatics S26 Repository

# Organism Bc394
<details>
<summary>Click to expand</summary>
  name species info stuff here
</details>

# Getting Started
1. Set up of VM using code
   
  ```
    wget https://www.cs.uky.edu/~acta225/CS485/vm_soft_setup.sh
  ```

2. Workshop materials
   
  ```
  wget https://www.cs.uky.edu/~acta225/CS485/workshop-materials.tar.xz
  ```

## Downloading Data
<details>
<summary>Click to expand</summary>
  1. Data was downloaded from the Farman Lab computer using the following code:
  
  ```
    scp -r ngs@10.163.188.11:Desktop/Bc394 ./sequences/
  ```

  2. Sequences were renamed 

  ```
  mv ./sequences/Be394/Bc394_CKDL250033905-1A_2335FYLT4_L1_1.fq.gz ./sequences/Be394/Be394_1.fq.gz
  mv ./sequences/Be394/Bc394_CKDL250033905-1A_2335FYLT4_L1_2.fq.gz ./sequences/Be394/Be394_2.fq.gz
  ```
  
  
</details>

## Checking Quality and Trimming
<details>
<summary>Click to expand</summary>
1. Fastqc was run on raw reads
  
```
    fastqc ./Bc394/Bc394_1.fq.gz ./Bc394/Bc394_2.fq.gz -o ~/sequences/
```

</details>
2. Adaptors were trimmed, see below for sequences:
<details>
<summary>Click to expand</summary>
 >PrefixNX/1
AGATGTGTATAAGAGACAG
>PrefixNX/2
AGATGTGTATAAGAGACAG
>Trans1
TCGTCGGCAGCGTCAGATGTGTATAAGAGACAG
>Trans1_rc
CTGTCTCTTATACACATCTGACGCTGCCGACGA
>Trans2
GTCTCGTGGGCTCGGAGATGTGTATAAGAGACAG
>Trans2_rc
CTGTCTCTTATACACATCTCCGAGCCCACGAGAC
>TruSeq_index3
GATCGGAAGAGCACACGTCTGAACTCCAGTCACTTAGGCATCTCGTATGC
>polyG
GGGGGGGGGGGGGGGGGGGG
</details>
3. Trimmomatic was run using this code:

  ```
java -jar trimmomatic-0.38.jar PE -threads 2 -phred33 -trimlog Br80_errorlog.txt ./Bc394/Bc394_1.fq.gz ./Bc394/Bc394_2.fq.gz ./Bc394/Bc394_1_paired.fastq ./Bc394/Bc394_1_unpaired.fastq ./Bc394/Bc394_2_paired.fastq ./Bc394/Bc394_2_unpaired.fastq ILLUMINACLIP:adaptors.fa:2:30:10 SLIDINGWINDOW:20:20 MINLEN:125
  ```

4. Fast QC was run again using this code on the paired and unpaired trimmed reads:
   
  ```
  fastqc /home/xrar222/sequences/Bc394/Bc394_1_unpaired.fastq /home/xrar222/sequences/Bc394/Bc394_1_paired.fastq /home/xrar222/sequences/Bc394/Bc394_2_unpaired.fastq /home/xrar222/sequences/Bc394/Bc394_2_paired.fastq -o /home/xrar222/sequences
  ```

  5. To determine the number of trimmed bases these codes were used (seperate for r1 and r2, added afterwards)

  ```
  grep LH00659 Bc394_1_paired.fastq -A 1 | grep -v "@LH00659" | grep -v "^-" | wc -m
  ```
  ```
  grep LH00659 Bc394_2_paired.fastq -A 1 | grep -v "@LH00659" | grep -v "^-" | wc -m
  ```

## Genome Assembly
  1. Transfer Data to MCC:

  2. Using Velvet:

  3. Using SPAdes:

  4. Looking at Bandage Plot
