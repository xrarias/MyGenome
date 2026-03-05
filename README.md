# Organism Bc394 Genome Assembly
CS485G: Applied Bioinformatics S26 Repository
<details>
<summary>Organism Information</summary>
  name species info stuff here
</details>

## Retrieving Dataset
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

## Assessing Sequence Quality And Trimming
-need to include fastQC code and versions of fastq and trimmomatic
<details>
<summary>FastQC pt. 1</summary>
1. FastQC was first used on the raw reads using the following code:
```
    fastqc ./Bc394/Bc394_1.fq.gz ./Bc394/Bc394_2.fq.gz -o ~/sequences/
```

</details>
<details>
<summary>Adaptors</summary>
1. Adaptors were trimmed, see below for sequences:
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
<details>
<summary>Trimmomatic</summary>
1. Trimmomatic 
  ```
java -jar trimmomatic-0.38.jar PE -threads 2 -phred33 -trimlog Br80_errorlog.txt ./Bc394/Bc394_1.fq.gz ./Bc394/Bc394_2.fq.gz ./Bc394/Bc394_1_paired.fastq ./Bc394/Bc394_1_unpaired.fastq ./Bc394/Bc394_2_paired.fastq ./Bc394/Bc394_2_unpaired.fastq ILLUMINACLIP:adaptors.fa:2:30:10 SLIDINGWINDOW:20:20 MINLEN:125
  ```
</details>
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

## Optimizing Genome Assembly 
  1. Transfer Data to MCC:

```
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_1_paired.fastq /project/farman_s26abt480/xrar222/Bc394
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_2_paired.fastq /project/farman_s26abt480/xrar222/Bc394
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_1_unpaired.fastq /project/farman_s26abt480/xrar222/Bc394
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_2_unpaired.fastq /project/farman_s26abt480/xrar222/Bc394

```
###Using Velvet:
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
### Using SPAdes:

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

Next, we wanted to look at:
number of contigs: 

```
grep -c '^>' Bc394_spades_assembly/scaffolds.fasta
```
Final genome size:

```
awk '/^>/ {next} {total += length($0)} END {print total}' scaffolds.fasta
```

N50: 

```
awk '/^>/ {if (l){print l}; l=0; next} {l+=length($0)} END {print l}' scaffolds.fasta \
| sort -nr \
| awk '{sum+=$1; a[NR]=$1} END {
    half=sum/2; running=0;
    for (i=1;i<=NR;i++){
        running+=a[i];
        if (running>=half){print a[i]; break}
    }
}'

```
Dr Farman and I then wondered if using Spades without the unpaired reads would work best, so I tested it on my genome. Spades then did a much better job of assembling. 

Spades new script only using paired reads looks like:

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

# now run spades using paired reads only

singularity run --app spades3155 /share/singularity/images/ccs/conda/amd-conda9-rocky8.sinf spades.py \
  --pe1-1 $readsdir/${MyGenome}_1_paired.fastq --pe1-2 $readsdir/${MyGenome}_2_paired.fastq \
  -o ${MyGenome}_spades_assembly
```

where directory (readsdir) is /project/farman_s26abt480/xrar222/Bc394 and MyGenome is Bc394

Next, we wanted to look at:
number of contigs: 

```
grep -c '^>' Bc394_spades_assembly_noUnpairedReads/scaffolds.fasta
```
Final genome size:

```
awk '/^>/ {next} {total += length($0)} END {print total}' scaffolds.fasta
```

N50: 

```
awk '/^>/ {if (l){print l}; l=0; next} {l+=length($0)} END {print l}' scaffolds.fasta \
| sort -nr \
| awk '{sum+=$1; a[NR]=$1} END {
    half=sum/2; running=0;
    for (i=1;i<=NR;i++){
        running+=a[i];
        if (running>=half){print a[i]; break}
    }
}'

```

It was determined that using the Spades assembly using only paired reads was best, so we looked into Bandage next

## Bandage Plot
downloaded Bandage from: https://rrwick.github.io/Bandage/
Wick R.R., Schultz M.B., Zobel J. & Holt K.E. (2015). Bandage: interactive visualisation of de novo genome assemblies. Bioinformatics, 31(20), 3350-3352.

used scp to download file: 
```
scp xrar222@mcc.uky.edu:/project/farman_s26abt480/xrar222/Bc394/Bc394_spades_assembly_noUnpairedReads/assembly_graph_with_scaffolds.gfa C:\Users\xrar222\Downloads​
```

Here were the images resulting from using Bandage as directed in Module 3:
<img width="308" height="311" alt="Bandange_SpadesGraphNode_Bc394_SNIP" src="https://github.com/user-attachments/assets/255e9900-d552-4959-8686-c9a234c2487f" />

<img width="723" height="431" alt="Bandange_SpadesGraphNode_Bc394" src="https://github.com/user-attachments/assets/d2d19b07-14a2-4660-b51a-c08507a59d4d" />


## Genome Post-Processing
The Spades Paired-Only genome was determined to be the best for continuing, so post processing occured on this genome.


a. Standardizing Headers
I moved /project/farman_s26abt480/xrar222/Bc394/Bc394_spades_assembly_noUnpairedReads/scaffolds.fasta to my working directory

```
cp /project/farman_s26abt480/xrar222/Bc394/Bc394_spades_assembly_noUnpairedReads/scaffolds.fasta /project/farman_s26abt480/xrar222/Bc394
```
Moved script to simplify fasta headers into working directory using code: 

```
cp /project/farman_s26abt480/SCRIPTs/SimpleFastaHeaders.pl /project/farman_s26abt480/xrar222/Bc394
```

Ran a perl script to rename the sequence headers to a standardized format

```
#!/usr/bin/perl

die "Usage: perl SimpleFastaHeaders.pl <dirname/filename>\n" if @ARGV < 1;

HEADER($ARGV[0]) if -f $ARGV[0];

& READ_DIR if -d $ARGV[0];

sub READ_DIR {

  ($indir = $ARGV[0]) =~ s/\/$//;

  opendir(FASTADIR, $indir) || die "There is not a directory of that name in the path you specified. Please try again\n\n";

  @FASTA = readdir(FASTADIR);

  $Success = 'no';

  foreach $Fasta (@FASTA) {

    print "$Fasta\n";

    if($Fasta =~ /fasta|fsa|fa|fna|mfa$/) {

      HEADER($Fasta);

      $Success='yes'

    }
    else {
      die "Sequence file(s) must end with fasta|fsa|fa|fna|mfa suffix\n";
    }
}

  close FASTADIR;

  die "No fasta files detected in specified directory. Genome assembly names must end in one of these suffixes: fasta|fsa|fa|fna|m$

}


sub HEADER {

  my $SeqNo = 0;

  my $Fasta = @_[0];

  @filepath = split(/\//, $Fasta);
$Genome_ID = $filepath[-1];

  $Genome_ID =~ s/_.+|\..+//;

  if(@ARGV == 2) {

    $Genome_ID = $ARGV[1];

    $outfile = $Genome_ID."_newheader.fasta";

    print "$outfile\n";

  }

  else {

    print "$Fasta\n";
    ($outfile = $Fasta) =~ s/.+\///;            # Strip off file path

    $outfile =~ s/_.*/_newheader.fasta/;

    print "$outfile\n";

  }

  open(FASTAOUT, '>', "$outfile") || die "Problem creating corrected genome file: $!\n";

  open(MAPOUT, '>', "$Genome_ID"."_contig_map.txt") || die "Problem creating contig mapping file\n" ;

  print MAPOUT "Old-genomeID\tNew-genomeID\n$oldGenome_ID\t$Genome_ID\nNew_ID\t\tOld_ID\n$oldGenome_ID\t$Genome_ID\n";

  print "Re-naming sequence headers for easy parsing in downstream applications\n";
open(FASTA, "$Fasta");

  while($Line = <FASTA>) {

    if($Line =~ /^>/) {

      $SeqNo ++;

      print FASTAOUT ">$Genome_ID"."_contig$SeqNo\n";

      $Line =~ s/^>//;

      print MAPOUT "$Genome_ID"."_contig$SeqNo\t$Line"

    }

    else {
      print FASTAOUT "$Line"

    }

  }

  close FASTA;

  close FASTAOUT;

  close MAPOUT;

  print   "\n########################################################\n\n".
        "Sequence headers have been converted and written to the file: $outfile\n".
        "Mapping between new and old sequence IDs was written to the file: $Genome_ID"."_contig_map.txt\n\n"
}

print "Conversion(s) finished.\n";
```
Submitted using

```
perl SimpleFastaHeaders.pl /project/farman_s26abt480/xrar222/Bc394/scaffolds.fasta Bc394
```

Now, headers that looked like >NODE_1_length_307413_cov_14.483598 now look like Bc394_contig1

b. Removing contigs <200bp and removing adaptor sequences

Copied processing script:
```
cp /project/farman_s26abt480/SLURM_SCRIPTs/GenomePostProcess.sh /project/farman_s26abt480/xrar222/Bc394
```
Script looks like: 
```
#!/bin/bash

#SBATCH --time 8:00:00
#SBATCH --job-name=GenomePostProcess
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16
#SBATCH --partition=normal
#SBATCH --mem=180GB
#SBATCH --mail-type ALL
#SBATCH -A cea_farman_s26abt480
#SBATCH --mail-type ALL
#SBATCH --mail-user farman@uky.edu,xrar222@uky.edu

echo "SLURM_NODELIST: "$SLURM_NODELIST
echo "PWD :" $PWD


# Arguments

genome=$1
prefix=${genome/_*/}
basename=${prefix/*\//}

cp -r /project/farman_s26abt480/RESOURCES .

# identify adaptors
module load ccs/conda/python/3.9.6
export FCS_LOC="/share/apps/amd/fcs/adaptor"
outdir=${basename}_FCS
mkdir $outdir
$FCS_LOC/run_fcsadaptor.sh --fasta-input $genome --output-dir $outdir \
 --euk --container-engine singularity --image $FCS_LOC/fcs-adaptor.sif

# trim/remove adaptors and contigs < 200 nt in length
export FCS_DEFAULT_IMAGE=RESOURCES/fcs-gx.sif
cat $genome | python3 RESOURCES/fcs.py clean genome \
  --action-report ${basename}_FCS/fcs_adaptor_report.txt --output ${basename}_final.fasta

```

Submitted:

```
sbatch /project/farman_s26abt480/xrar222/Bc394/GenomePostProcess.sh ./Bc394_newheader.fasta
```

## Final Assembly Data

Used Seqkit 
```
 singularity run --app seqkit2900 /share/singularity/images/ccs/conda/amd-conda19-rocky9.sinf seqkit stats Bc394_final.fasta
```
Output was:
```
file               format  type  num_seqs     sum_len  min_len   avg_len  max_len
Bc394_final.fasta  FASTA   DNA      2,960  43,534,695      200  14,707.7  307,413
```
And N50 was calculated using:
```
awk '/^>/ {if (l) print l; l=0; next} {l+=length($0)} END {print l}' Bc394_final.fasta | sort -nr | awk '{t+=$1; a[NR]=$1} END {h=t/2; s=0; for(i=1;i<=NR;i++){s+=a[i]; if(s>=h){print a[i]; exit}}}'
```

Therefore:
a. Genome Size: 43,534,695
b. Number of Contigs: 2,960
c. N50: 71,435

Fold Coverage was calculated:
Total # of bases / Genome Size

## Genome Quality Assessment Using BUSCO

Next we ran Busco using the ascomycota_odb10 lineage dataset. 

```
#!/bin/bash

#SBATCH --time 8:00:00
#SBATCH --job-name=busco
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16
#SBATCH --partition=normal
#SBATCH --mem=180GB
#SBATCH --mail-type ALL
#SBATCH -A cea_farman_s26abt480
#SBATCH --mail-type ALL
#SBATCH --mail-user xrar222@uky.edu

echo "SLURM_NODELIST: "$SLURM_NODELIST
echo "PWD :" $PWD

in=$1
out=${in/\.fasta/}_busco

singularity run --app busco570 /share/singularity/images/ccs/conda/amd-conda14-rocky8.sinf busco \
 --in $in --out $out --mode genome --lineage_dataset ascomycota_odb10 -f
```
Submission was:
```
sbatch BuscoSingularity.sh ./Bc394_final.fasta
```
Output was:
```
# BUSCO version is: 5.7.0
# The lineage dataset is: ascomycota_odb10 (Creation date: 2024-01-08, number of genomes: 365, number of BUSCOs: 1706)
# Summarized benchmarking in BUSCO notation for file /project/farman_s26abt480/xrar222/Bc394_final.fasta
# BUSCO was run in mode: euk_genome_min
# Gene predictor used: miniprot

        ***** Results: *****

        C:98.3%[S:98.0%,D:0.3%],F:0.2%,M:1.5%,n:1706,E:3.5%
        1677    Complete BUSCOs (C)     (of which 58 contain internal stop codons)
        1672    Complete and single-copy BUSCOs (S)
        5       Complete and duplicated BUSCOs (D)
        4       Fragmented BUSCOs (F)
        25      Missing BUSCOs (M)
        1706    Total BUSCO groups searched

Assembly Statistics:
        2960    Number of scaffolds
        3058    Number of contigs
        43534695        Total length
        0.015%  Percent gaps
        71 KB   Scaffold N50
        64 KB   Contigs N50


Dependencies and versions:
        hmmsearch: 3.1
        bbtools: 39.06
        miniprot_index: 0.13-r248
        miniprot_align: 0.13-r248
        python: sys.version_info(major=3, minor=7, micro=12, releaselevel='final', serial=0)
        busco: 5.7.0
```
## Using BLAST, Visualizing Genes, Performing Gene Predictions
