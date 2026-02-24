# Organism Bc394
CS485G: Applied Bioinformatics S26 Repository
<details>
<summary>Organism Information</summary>
  name species info stuff here
</details>

# Getting Started
<details>
  <summary>Setting Up The Workspace</summary>
1. Set up of VM using code
   
  ```
    wget https://www.cs.uky.edu/~acta225/CS485/vm_soft_setup.sh
  ```

2. Workshop materials
   
  ```
  wget https://www.cs.uky.edu/~acta225/CS485/workshop-materials.tar.xz
  ```
</details>
<details>
<summary>Downloading Data</summary>
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

# Checking Quality and Trimming
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

# Genome Assembly
  1. Transfer Data to MCC:

```
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_1_paired.fastq /project/farman_s26abt480/xrar222/Bc394
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_2_paired.fastq /project/farman_s26abt480/xrar222/Bc394
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_1_unpaired.fastq /project/farman_s26abt480/xrar222/Bc394
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_2_unpaired.fastq /project/farman_s26abt480/xrar222/Bc394

```
  2. Using Velvet:
  Used Velvet Advisor (https://dna.med.monash.edu/~torsten/velvet_advisor/) to determine 77 was the suggested kmer length (based on 5.39 million reads, 150bp length, paired end, 20 fold coverage,and a 40Mbp genome size)
    Assembly script:
<details>
<summary>Click to expand</summary>
  
#!/bin/bash

#SBATCH --time 48:00:00
#SBATCH --job-name=VelvetOptimiser
#SBATCH --nodes=1
#SBATCH --ntasks=8
#SBATCH --cpus-per-task=16
#SBATCH --partition=short
#SBATCH --mem=180GB
#SBATCH --mail-type ALL
#SBATCH -A cea_farman_s26abt480
#SBATCH --mail-type ALL
#SBATCH --mail-user farman@uky.edu,xrar222@uky.edu

echo "SLURM_NODELIST: "$SLURM_NODELIST

# define variables
  strainID=$1

  lowK=$2

  highK=$3

  step=$4

# make a directory for assemblies
  mkdir $strainID

# copy paired reads into directory
  cp $strainID*1_paired*f*q* $strainID/
  cp $strainID*2_paired*f*q* $strainID/

# change into directory
  cd $strainID

# create hard-coded read names
  cp $strainID*1_paired*f*q* forward.fq
  cp $strainID*2_paired*f*q* reverse.fq

# run velvetoptimiser in singularity
  singularity run --app perlvelvetoptimiser226 /share/singularity/images/ccs/conda/amd-conda2-centos8.sinf VelvetOptimiser.pl \
  -s $lowK -e $highK -x $step -d velvet_${strainID}_${lowK}_${highK}_${step}_noclean -o ' -clean no' -f ' -shortPaired -fastq -separate forward.fq reverse.fq'

# remove read files with hard-coded names
  rm forward.fq
  rm reverse.fq
  
</details>

Submitted job using:

```
sbatch velvetoptimiser.sh Bc394 37 117 10
```
where 37 is the suggested kmer length minus 40 and 117 is the suggested kmer length plus 40 for the low and high ranges, and 10 is the step size.

then, to get to the better kmer length based off the optimal velvet hash value of 97, where 87 is the low and 107 is the high, with a step size of 2. The original write out directory was renamed to first_run to prevent overwriting

```
sbatch velvetoptimiser.sh Bc394 87 107 2
```

  4. Using SPAdes:

copied slurm script using 

```
cp /project/farman_s26abt480/SLURM_SCRIPTs/spades.sh /project/farman_s26abt480/xrar222/Bc394

```

Spades script looks like:

```
#!/bin/bash

#SBATCH --time 48:00:00
#SBATCH --job-name=spades
#SBATCH --nodes=1
#SBATCH --ntasks=4
#SBATCH --cpus-per-task=8
#SBATCH --partition=short
#SBATCH --mem=500GB
#SBATCH --mail-type ALL
#SBATCH -A cea_farman_s26abt480
#SBATCH --mail-type ALL
#SBATCH --mail-user farman@uky.edu,xrar222@uky.edu

echo "SLURM_NODELIST: "$SLURM_NODELIST

readsdir=$1

MyGenome=$2

# now run spades using paired and single end reads

singularity run --app spades3155 /share/singularity/images/ccs/conda/amd-conda9-rocky8.sinf spades.py \
  --pe1-1 $readsdir/${MyGenome}_1_paired.fastq --pe1-2 $readsdir/${MyGenome}_2_paired.fastq \
  --pe1-s $readsdir/${MyGenome}_1_unpaired.fastq --pe1-s $readsdir/${MyGenome}_2_unpaired.fastq \
  -o ${MyGenome}_spades_assembly
```

where directory (readsdir) is /project/farman_s26abt480/xrar222/Bc394 and MyGenome is Bc394




  6. Looking at Bandage Plot
downloaded Bandage from: https://rrwick.github.io/Bandage/
Wick R.R., Schultz M.B., Zobel J. & Holt K.E. (2015). Bandage: interactive visualisation of de novo genome assemblies. Bioinformatics, 31(20), 3350-3352.

used scp to download file: 

