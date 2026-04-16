# Organism Bc394 Genome Assembly

CS485G: Applied Bioinformatics S26 Repository

_.~"~._.~"~._.~"~._.~"~._.~"~._.~"~._.~"~._.~"~._.~"~.__.~"~._.~"~._.~"~._.~"~._.~"~._.~"~._.~"~._.~"~._.~"~.__.~"~._.~"~._.~"~._.~"~._.~"~._.~"~._.~"~._.~"~._.~"~._


## Getting Started
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
  <details>
<summary>Organism Information</summary>

```
Organism: Pyricularia oryzae

Host of Organism: Bromus catharticus leaf

Name of Isolate/Genome Individual: Bc394

Year Collected: 2024

Geographic Location of Collection: Brazil: Yaguarón

Bioproject: PRJNA926786

Biosample ID: SAMN55078751

Library Strategy: WGS

Library Source: Genomic

Library Selection: Random

Library Layout: Paired

Platform: Illumina

Instrument: NovaSeq X

Extraction: SparQ DNA Library Prep

```
  
</details>

## Assessing Sequence Quality And Trimming
<details>
<summary>FastQC pt. 1</summary>
1. FastQC (v0.10.1) was first used on the raw reads using the following code:
  
```
    fastqc ./Bc394/Bc394_1.fq.gz ./Bc394/Bc394_2.fq.gz -o ~/sequences/
```

2. Orange and red quality messages from FastQC for the raw data are below:
  Read 1:

```
    Per tile sequence quality (yellow)
    Per base sequence content (red)
    Per sequence GC content (yellow)
    Overrepresented sequences (yellow)
    Adaptor content (red)
```

  <img width="959" height="362" alt="Read1RawFastQCReport Bc394" src="https://github.com/user-attachments/assets/9138ba46-4ca6-48bd-8384-f314a5f9d791" />

```
  Read 2:
    Per base sequence content (red)
    Per sequence GC content (yellow)
    Overrepresented sequences(yellow)
    Adaptor content (red)
```

  <img width="954" height="357" alt="Read2RawFastQCReport Bc394" src="https://github.com/user-attachments/assets/ea9bf506-77d4-4e0b-b5e4-3e1c2a6bc1b0" />

</details>
<details>
<summary>Adaptors</summary>
1. Adaptors were trimmed, see below for sequences:

  ```
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
```
</details>
<details>
<summary>Trimmomatic</summary>
1. Trimmomatic (v0.38) was run to trim the data.
  
  ```
java -jar trimmomatic-0.38.jar PE -threads 2 -phred33 -trimlog Br80_errorlog.txt ./Bc394/Bc394_1.fq.gz ./Bc394/Bc394_2.fq.gz ./Bc394/Bc394_1_paired.fastq ./Bc394/Bc394_1_unpaired.fastq ./Bc394/Bc394_2_paired.fastq ./Bc394/Bc394_2_unpaired.fastq ILLUMINACLIP:adaptors.fa:2:30:10 SLIDINGWINDOW:20:20 MINLEN:125
  ```

</details>
<details>
<summary>FastQC pt. 2</summary>
1. Fast QC (v0.10.1) was run again using this code on the paired and unpaired trimmed reads:
   
  ```
  fastqc /home/xrar222/sequences/Bc394/Bc394_1_unpaired.fastq /home/xrar222/sequences/Bc394/Bc394_1_paired.fastq /home/xrar222/sequences/Bc394/Bc394_2_unpaired.fastq /home/xrar222/sequences/Bc394/Bc394_2_paired.fastq -o /home/xrar222/sequences
  ```

2. Orange and red quality messages from FastQC for the trimmed data are below:

```
  Read 1 Paired:
   Per tile sequence quality (yellow)
   Per base sequence content (red)
   Per sequence GC content (yellow)
   Sequence length distribution (yellow)
```

<img width="955" height="369" alt="Read1PairedTrimmedFastQCReport Bc394" src="https://github.com/user-attachments/assets/3907a69a-1565-46d5-8fad-22adab4ed9da" />

   ```
  Read 1 Unpaired:
   Per tile sequence quality (yellow)
   Per base sequence content (red)
   Per sequence GC content (yellow)
   Sequence length distribution (yellow)
```

<img width="959" height="364" alt="Read1UnpairedTrimmedFastQCReport Bc394" src="https://github.com/user-attachments/assets/4212e10b-1feb-41ee-8090-bb37a7824a43" />

  ```
  Read 2 Paired:
   Per tile sequence quality (yellow)
   Per base sequence content (red)
   Per sequence GC content (yellow)
   Sequence length distribution (yellow)
   Adapter content (yellow)
```

<img width="956" height="365" alt="Read2PairedTrimmedFastQCReport Bc394" src="https://github.com/user-attachments/assets/dbb00a54-51ed-47ca-a01b-5c1f4209a1c4" />

   ```
  Read 2 Unpaired:
   Per tile sequence quality (yellow)
   Per base sequence content (red)
   Per sequence GC content (yellow)
   Sequence length distribution (yellow)
   Adapter content (red)
```

<img width="959" height="371" alt="Read2UnpairedTrimmedFastQCReport Bc394" src="https://github.com/user-attachments/assets/2d5a8a53-ab3e-4c97-bf73-cef17af6dd96" />

</details>

<details>
<summary>Trimmed Bases</summary>
1. To determine the number of trimmed bases these codes were used (seperate for r1 and r2, added afterwards)

  ```
  grep LH00659 Bc394_1_paired.fastq -A 1 | grep -v "@LH00659" | grep -v "^-" | wc -m
  ```
  ```
  grep LH00659 Bc394_2_paired.fastq -A 1 | grep -v "@LH00659" | grep -v "^-" | wc -m
  ```

Cleaned reads used for assembly (paired only; single end): 5,391,917	

Total bases in cleaned reads (paired only; R1 + R2): 1,621,261,296
</details>

## Optimizing Genome Assembly 
<details>
<summary>Transferring Data</summary>
  1. From VM to MCC:

```
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_1_paired.fastq /project/farman_s26abt480/xrar222/Bc394
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_2_paired.fastq /project/farman_s26abt480/xrar222/Bc394
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_1_unpaired.fastq /project/farman_s26abt480/xrar222/Bc394
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/sequences/Bc394/Bc394_2_unpaired.fastq /project/farman_s26abt480/xrar222/Bc394
```
</details>

<details>
<summary>Using Velvet 2.2.6 to Assemble</summary>
  
1. Used Velvet Advisor (https://dna.med.monash.edu/~torsten/velvet_advisor/) to determine 77 was the suggested kmer length (based on 5.39 million reads, 150bp length, paired end, 20 fold coverage, and a 40Mbp genome size)

2. Velvet Assembly Script using 10 step process:
  
 ``` 
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
  
```

3. Submitted job using the following command where the high and low ranges are dictated by 37 as the suggested kmer length minus 40 and 117 as the suggested kmer length plus 40. The final part of the command, 10, is the step size.

```
sbatch velvetoptimiser.sh Bc394 37 117 10
```

4. The result of this first Velvet run (step 10) led to determining the optimal Velvet hash value was 97. To discover the actual ideal kmer length, 87 was used as a low and 107 was used as a high, with a step size of 2. The original write out directory was renamed to first_run to prevent overwriting with this second run. 

```
sbatch velvetoptimiser.sh Bc394 87 107 2
```

5. Next, the outcome of the step 10 and step 2 Velvet assemblies were compared:

```
   Velvet step 10:
      # of contigs: 8,790
      N50: 13,316
      Genome Size: 44,039,166

   Velvet step 2:
      # of contigs: 7,409
      N50: 16,969
      Genome Size: 45,234,963
```
</details>
<details>
<summary>Using SPAdes 3.15.5 to Assemble</summary>

1. The SPAdes script used was as follows, where, when submitted, the directory (readsdir) is /project/farman_s26abt480/xrar222/Bc394 and the MyGenome is Bc394.

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

2. Next, we wanted to look at the following points for the assembly:

Number of contigs: 

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
3. Dr. Mark Farman and I then wondered if using SPAdes without the unpaired reads would work best, so I tested it on my genome. The new SPAdes script using only paired reads was as follows:

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

4. Then, we looked at:

Number of contigs: 

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

5. The outcome of the paired + unpaired vs paired SPAdes assemblies were compared:
```
   SPAdes (Paired + Unpaired):
      # of contigs: 17,655
      N50: 58,931
      Genome Size: 45,234,963

   SPAdes (Paired Only):
      # of contigs: 5,613
      N50: 71,381
      Genome Size: 43,858,169
```

6. It was determined that using the SPAdes assembly using only paired reads was signficantly better when comparing all assemblers. The final SPAdes assembly with only paired reads was chosen to continue work on. 

<img width="665" height="142" alt="image" src="https://github.com/user-attachments/assets/411b638d-3662-45f1-8f26-343c38d0821b" />

</details>

## Bandage Plot
<details>
<summary>Bandage</summary>
  
1. Bandage was downloaded from: https://rrwick.github.io/Bandage/ onto my local machine.
   
```
Wick R.R., Schultz M.B., Zobel J. & Holt K.E. (2015). Bandage: interactive visualisation of de novo genome assemblies. Bioinformatics, 31(20), 3350-3352.
```

3. Used scp to download file of assembly graph to my local machine for examination:
```
scp xrar222@mcc.uky.edu:/project/farman_s26abt480/xrar222/Bc394/Bc394_spades_assembly_noUnpairedReads/assembly_graph_with_scaffolds.gfa C:\Users\19042\Downloads​
```

The following images resulted from using Bandage as directed in Module 3 in ABT485G, where we examined everything and then highlighted a nodeto explore further and gain understaning of how Bandage works:


<img width="308" height="311" alt="Bandange_SpadesGraphNode_Bc394_SNIP" src="https://github.com/user-attachments/assets/255e9900-d552-4959-8686-c9a234c2487f" />

<img width="723" height="431" alt="Bandange_SpadesGraphNode_Bc394" src="https://github.com/user-attachments/assets/d2d19b07-14a2-4660-b51a-c08507a59d4d" />

</details>

## Genome Post-Processing
<details>
<summary>Standardizing Headers</summary>
1. I moved /project/farman_s26abt480/xrar222/Bc394/Bc394_spades_assembly_noUnpairedReads/scaffolds.fasta to my working directory

```
cp /project/farman_s26abt480/xrar222/Bc394/Bc394_spades_assembly_noUnpairedReads/scaffolds.fasta /project/farman_s26abt480/xrar222/Bc394
```
2. Moved script to simplify fasta headers into working directory using code: 

```
cp /project/farman_s26abt480/SCRIPTs/SimpleFastaHeaders.pl /project/farman_s26abt480/xrar222/Bc394
```

3. Ran a perl script to rename the sequence headers to a standardized format:

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
4. Submitted using the following command, and now, headers that looked like >NODE_1_length_307413_cov_14.483598 now look like Bc394_contig1

```
perl SimpleFastaHeaders.pl /project/farman_s26abt480/xrar222/Bc394/scaffolds.fasta Bc394
```
</details>
<details>
<summary>Removing Contigs <200bp + Removing Adapter Sequences</summary>

1.Copied processing script:
```
cp /project/farman_s26abt480/SLURM_SCRIPTs/GenomePostProcess.sh /project/farman_s26abt480/xrar222/Bc394
```
2. Script looks like: 
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

3. Submitted using the following code:

```
sbatch /project/farman_s26abt480/xrar222/Bc394/GenomePostProcess.sh ./Bc394_newheader.fasta
```
</details>

<details>
<summary>Final Assembly Data</summary>

1. Used Seqkit to inspect statistics on the final assembly:
```
 singularity run --app seqkit2900 /share/singularity/images/ccs/conda/amd-conda19-rocky9.sinf seqkit stats Bc394_final.fasta
```
2. The output was:
```
file               format  type  num_seqs     sum_len  min_len   avg_len  max_len
Bc394_final.fasta  FASTA   DNA      2,960  43,534,695      200  14,707.7  307,413
```
3. The N50 was calculated using:
```
awk '/^>/ {if (l) print l; l=0; next} {l+=length($0)} END {print l}' Bc394_final.fasta | sort -nr | awk '{t+=$1; a[NR]=$1} END {h=t/2; s=0; for(i=1;i<=NR;i++){s+=a[i]; if(s>=h){print a[i]; exit}}}'
```

4. Fold coverage was calculated as:
```
   Total # of bases / Genome Size
```

4. The final genome data is as follows:

```
a. Genome Size: 43,534,695
b. Number of Contigs: 2,960
c. N50: 71,435
d. Fold Coverage: 37.24
```
</details>

## Genome Quality Assessment
<details>
<summary>BUSCO</summary>
1. BUSCO was then ran using the ascomycota_odb10 lineage dataset. 

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

2. Submission was completed using this command:
   
```
sbatch BuscoSingularity.sh ./Bc394_final.fasta
```

3. The output was:

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
</details>
<details>
<summary>BLAST for Mitochondrial Genes</summary>
  
1. BLAST was used to find mitochondrial sequences in the genome using the following script:
  
```
#!/bin/bash

#SBATCH --time 8:00:00
#SBATCH --job-name=BLASTn
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16
#SBATCH --partition=normal
#SBATCH --mem=180GB
#SBATCH --mail-type ALL
#SBATCH -A cea_farman_s26abt480
#SBATCH --mail-type ALL
#SBATCH --mail-user xrar222@uky.edu

singularity run --app blast2120 /share/singularity/images/ccs/conda/amd-conda1-centos8.sinf blastn -query MoMitochondrion.fasta -subject Bc394_final.fasta -evalue 1e-50 -max_target_seqs 20000 -outfmt '6 qseqid sseqid slen length qstart qend sstart send btop' -out MoMitochondrion.Bc394.BLAST
```
2. I then sorted to only find the sequences that are 90% match or above:
```
awk '$4/$3 >= 0.9 {print $2 ",mitochondrion"}' MoMitochondrion.Bc394.BLAST > Bc394_mitochondrion.csv
```
3. Next a different file was created to only find sequencines 90% match or below, so we can assess the potential split parts of the mitochondria:

```
awk '$4/$3 <= 0.9 {print}' MoMitochondrion.Bc394.BLAST > Bc394_short_mitochondrial_hits.txt
```
4. In order to figure out what mitochondrial sequences had been split, the following information was used, where I was loking for where two separate sequences line up to the contig (query is mito, contig is subject).

```
qseqid 		sseqid 			slen 	length 	qstart 	qend 	sstart 	send 	btop
MoMito.70-15    Bc394_contig1122        3166    1638    30377   32013   3166    1529    35AT950-A651
MoMito.70-15    Bc394_contig1122        3166    1428    31937   33364   1428    1       90TA337AC999
MoMito.70-15    Bc394_contig1462        1076    550     16830   17379   550     1       550
MoMito.70-15    Bc394_contig1462        1076    534     16846   17379   543     1076    534
MoMito.70-15    Bc394_contig1681        650     344     20997   21340   1       343     340C-3
MoMito.70-15    Bc394_contig1681        650     344     20997   21340   650     308     340C-3
```

```
slen 		Full length of the subject sequence
length 		Length of the aligned segment (HSP)
qstart 		Start coordinate on query
qend 		End coordinate on query
sstart 		Start coordinate on subject
send		End coordinate on subject
```
5. Based on this data and analysis, the following contigs were added to the mitochondrial .csv because contig1122 had a 101 base difference (possible gap), and contig1462 and contig1681 overlapped. 

```
Bc394_contig1122,mitochondrion
Bc394_contig1462,mitochondrion
Bc394_contig1681,mitochondrion
```
6. This is the mitochondiral contig file:[Bc394_mitochondrion.csv](https://github.com/user-attachments/files/26286668/Bc394_mitochondrion.csv)

```
Bc394_contig829,mitochondrion
Bc394_contig1011,mitochondrion
Bc394_contig1208,mitochondrion
Bc394_contig1287,mitochondrion
Bc394_contig1292,mitochondrion
Bc394_contig1294,mitochondrion
Bc394_contig1385,mitochondrion
Bc394_contig1412,mitochondrion
Bc394_contig1478,mitochondrion
Bc394_contig1481,mitochondrion
Bc394_contig1513,mitochondrion
Bc394_contig1576,mitochondrion
Bc394_contig1631,mitochondrion
Bc394_contig1644,mitochondrion
Bc394_contig1684,mitochondrion
Bc394_contig1700,mitochondrion
Bc394_contig1798,mitochondrion
Bc394_contig1806,mitochondrion
Bc394_contig1863,mitochondrion
Bc394_contig1915,mitochondrion
Bc394_contig2470,mitochondrion
Bc394_contig1122,mitochondrion
Bc394_contig1462,mitochondrion
Bc394_contig1681,mitochondrion
```
</details>

## Visualizing Genes, Performing Gene Predictions
<details>
<summary>Setting up the HMM (Hidden Markov Model)</summary>

  1.Files were transferred from MCC to VM using the following code:

```
scp /project/farman_s26abt480/RESOURCES/B71Ref2_a0.3.gff3 xrar222@xrar222.cs.uky.edu:/home/xrar222/genes/snap
```

2. The genome fasta sequence was appended to the gff3 file using the following command:

```
echo '##FASTA' | cat B71Ref2_a0.3.gff3 - B71Ref2.fasta > B71Ref2.gff3
```

3. The MAKER annotations were converted to ZFF for using in SNAP:

```
maker2zff B71Ref2.gff3
```

4. Information on the annotations was gathered using fathom:

```
fathom genome.ann genome.dna -gene-stats
```
5. Which resulted in the following output:

```
MODEL8478 skipped due to errors
8 sequences
0.497445 avg GC fraction (min=0.480103 max=0.512185)
12378 genes (plus=6275 minus=6103)
3479 (0.281063) single-exon
8899 (0.718937) multi-exon
567.119629 mean exon (min=1 max=17982)
103.271423 mean intron (min=11 max=2968)
```

6. Genome regions containing unique genes were extracted using the following command, where 1000 indicates looking at up to 1000bp of intergenic sequence before and after each gene.

```
fathom genome.ann genome.dna -categorize 1000
```

7. Gene statistics were looked at again, this time on the unique and non-overlapping genes without alternative splicing:

```
fathom uni.ann uni.dna -gene-stats
```

8. Where the result was:

```
10732 sequences
0.524754 avg GC fraction (min=0.357478 max=0.659509)
10732 genes (plus=5451 minus=5281)
3370 (0.314014) single-exon
7362 (0.685986) multi-exon
586.630676 mean exon (min=6 max=13918)
102.167183 mean intron (min=31 max=2105)
```

9. The genome, transcript, and protein sequences were then exported along with 1000bp of "context" on either side, and being careful to flip genes on the reverse strand (using the -plus flag).

```
fathom uni.ann uni.dna -export 1000 -plus
```
10. Genestats was used to look at the export.ann and export.dna files. 

```
fathom export.ann export.dna -gene-stats
```
11. Where the ouput was:

```
10732 sequences
0.524754 avg GC fraction (min=0.357478 max=0.659509)
10732 genes (plus=10732 minus=0)
3370 (0.314014) single-exon
7362 (0.685986) multi-exon
586.630676 mean exon (min=6 max=13918)
102.167183 mean intron (min=31 max=2105)
```

12. The forge command was used to train the HMM.

```
forge export.ann export.dna
```

13. hmm-assembler was used to condense the forge created files to a single file for SNAP.

```
hmm-assembler.pl Moryzae . > Moryzae.hmm
```

14. The final genome file was transferred from the MCC to the VM:

```
scp /project/farman_s26abt480/xrar222/FinalSubmittedToNCBI/Bc394_final.fasta xrar222@xrar222.cs.uky.edu:/home/xrar222/genes/snap/
```
</details>
<details>
<summary>Running Snap</summary>
1. SNAP was run using the parameter file on the genome file, and directed to the Bc394-snap.zff file.

```
snap-hmm Moryzae.hmm Bc394_final.fasta > Bc394-snap.zff
```

2. Fathom was used to examine the .zff and .fasta files. 

```
fathom Bc394-snap.zff Bc394_final.fasta -gene-stats
```

3. The output of fathom was:

```
2960 sequences
0.467022 avg GC fraction (min=0.148098 max=0.741935)
12672 genes (plus=6380 minus=6292)
4193 (0.330887) single-exon
8479 (0.669113) multi-exon
586.166931 mean exon (min=4 max=19581)
106.837311 mean intron (min=4 max=1410)
```
4. The number of genes found by snap was verified as 12,672 using this code:

```
awk '{print $9}' Bc394-snap.gff2 | sort -u | wc -l
```

5. A GFF2 format was created using the following command:

```
snap-hmm Moryzae.hmm Bc394_final.fasta -gff > Bc394-snap.gff2
```
</details> 
<details>
<summary>Running Augustus</summary>

1. Augustus was run using the following code where: Magnaporthe grisea was used as the organism parameter file, gff3 was specified as the output file, genes were predicted on each strand seperately, a progress bar was used, and the information was redirected to Bc394-augustus.gff3.

```
augustus --species=magnaporthe_grisea --gff3=on --singlestrand=true --progress=true Bc394_final.fasta > Bc394-augustus.gff3
```

2. The number of genes found by Augustus was verified as 17,578 using this code:

```
grep "^# start gene" Bc394-augustus.gff3 | wc -l
```

</details>
<details>
<summary>Running MAKER</summary>

1. The Moryzae.hmm, ncbi-cds-Magnaporthe_organism.fasta, ncbi-protein-Magnaporthe_organism.fasta, and final BC394 fasta was transferred from the VM to the MCC or copied from a different location (from within the MCC):

```
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/genes/snap/Moryzae.hmm /project/farman_s26abt480/xrar222/Bc394/MAKER
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/genes/maker/genbank/ncbi-cds-Magnaporthe_organism.fasta /project/farman_s26abt480/xrar222/Bc394/MAKER
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/genes/maker/genbank/ncbi-protein-Magnaporthe_organism.fasta /project/farman_s26abt480/xrar222/Bc394/MAKER
cp /project/farman_s26abt480/xrar222/FinalSubmittedToNCBI/Bc394_final.fasta /project/farman_s26abt480/xrar222/Bc394/MAKER
```

2. MAKER configuration files were made using this code:

```
singularity exec /share/singularity/images/ccs/MAKER/amd-maker-debian10.sinf maker -CTL
```

3. maker_opts.ctl was opened using nano and the following changes were made:

```
genome=/project/farman_s26abt480/xrar222/Bc394/MAKER/Bc394_final.fasta
model_org=
repeat_protein=
snaphmm=/project/farman_s26abt480/xrar222/Bc394/MAKER/Moryzae.hmm
augustus_species=magnaporthe_grisea
keep_preds=1
protein=/project/farman_s26abt480/xrar222/Bc394/MAKER/ncbi-protein-Magnaporthe_organism.fasta
```

4. The Maker script was copied to the working directory.

```
cp /project/farman_s26abt480/SLURM_SCRIPTs/maker.sh /project/farman_s26abt480/xrar222/Bc394/MAKER
```

6. The Maker script looks like:
  
```
#!/bin/bash

#SBATCH --time 48:00:00
#SBATCH --job-name=maker
#SBATCH --nodes=1
#SBATCH --ntasks=16
#SBATCH --cpus-per-task=8
#SBATCH --partition=normal
#SBATCH --mem=500GB
#SBATCH -A cea_farman_s26abt480
#SBATCH --mail-type ALL
#SBATCH --mail-user xrar222@uky.edu

genome=$1

singularity exec /share/singularity/images/ccs/MAKER/amd-maker-debian10.sinf maker --genome $genome 2>&1 | tee -- ${genome/\.fasta}_maker.log
```

8. The Maker script was submitted with the following command:

```
sbatch maker.sh /project/farman_s26abt480/xrar222/Bc394/MAKER/Bc394_final.fasta
```

9. The Maker output was combined into a GFF file using the following script:

```
#!/bin/bash

#SBATCH --time 1:00:00
#SBATCH --job-name=makermerge
#SBATCH --partition=normal
#SBATCH -A cea_farman_s26abt480


singularity exec /share/singularity/images/ccs/MAKER/amd-maker-debian10.sinf gff3_merge -d Bc394_final.maker.output/Bc394_final_master_datastore_index.log -o Bc394-maker.gff3
```
10a. The number of genes found by MAKER was verified as 13,098 using this code:

```
grep -P "\tgene\t" Bc394-maker.gff3 | wc -l
```
  10b . This code also works for verifying gene count:

```
awk '$3 == "gene"' Bc394-maker.gff3 | wc -l
```

</details>
<details>
<summary>IGV</summary>
1. IGV was installed to my local machine.
2. The following files were downloaded onto my local machine, inititating inside the local machine using Powershell.

```
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/genes/augustus/Bc394-augustus.gff3 C:\Users\19042\Downloads\IGVforClass
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/genes/augustus/Bc394_final.fasta C:\Users\19042\Downloads\IGVforClass
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/genes/snap/Bc394-snap.gff2 C:\Users\19042\Downloads\IGVforClass
scp xrar22@mcc.uky.edu:/project/farman_s26abt480/xrar222/Bc394/MAKER/Bc394-maker.gff3 C:\Users\19042\Downloads\IGVforClass
```

3. IGV and the genome was explored to examine differences and similarities between the different programs.
   
3a. A screen shot of IGV browser window showing an example of a gene predicted only by snap
<img width="515" height="351" alt="GeneSnapOnly" src="https://github.com/user-attachments/assets/a8fbb967-c067-4ce1-ab4c-eeb8dadc37ff" />

3b. A screen shot of IGV browser window showing an example of a gene predicted only by AUGUSTUS
<img width="845" height="540" alt="AugustusOnlyfinal" src="https://github.com/user-attachments/assets/1beede5b-6f60-4dc5-a50f-949ba2080654" />

3c. A screen shot of IGV browser window showing an example of a gene predicted only by MAKER
<img width="787" height="470" alt="image" src="https://github.com/user-attachments/assets/552190c6-7c6d-4c40-bb2e-2f5c0e1e65bc" />

3d. A screen shot of a gene where snap and AUGUSTUS predict the same exon/intron structure
<img width="763" height="334" alt="BothGene" src="https://github.com/user-attachments/assets/93667ae1-6adc-4d52-bf22-b3b413764188" />

3e. Screenshots of genes where snap and AUGUSTUS predict a different exon/intron structure
<img width="946" height="317" alt="GeneAugustusOnly" src="https://github.com/user-attachments/assets/50bb8a27-d6b9-4ebd-922f-dbcaee47e855" />

<img width="775" height="335" alt="image" src="https://github.com/user-attachments/assets/ea062fae-26fb-4cb7-a45a-cf0ffaf492fd" />

3f. A screen shot where a gene was successfully predicted by snap, AUGUSTUS, and MAKER with external evidence that the prediction is correct

<img width="601" height="555" alt="image" src="https://github.com/user-attachments/assets/2e430e9d-cb05-4ee3-834e-f22f95069693" />


</details>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
<details>
<summary>NEIGH!</summary>
  
```
                           ___________ _
                      __/   .::::.-'-(/-/)
                    _/:  .::::.-' .-'\/\_`,
                   /:  .::::./   -._-.  d\|
                    /: (""""/    '.  (__/||
                     \::).-'  -._  \/ \\/\|
             __ _ .-'`)/  '-'. . '. |  (i_O
         .-'      \       -'      '\|
    _ _./      .-'|       '.  (    \\
 .-'   :      '_  \         '-'\  /|/
/      )\_      '- )_________.-|_/^\
(   .-'   )-._-:  /        \(/\'-._ `.
 (   )  _//_/|:  /          `\()   `\_\
  ( (   \()^_/)_/             )/      \\
   )     \\ \(_)             //        )\
         _o\ \\\            (o_       |__\
         \ /  \\\__          )_\
               ^)__\
```
</details>
