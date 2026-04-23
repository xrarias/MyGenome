# Organism Bc394 Genome Assembly

CS485G: Applied Bioinformatics S26 Repository

<><><><><><><><><><><><><><><><><><><><><><><><><><><><><><><><><><><>

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

```
Read 1:
    Per tile sequence quality (yellow)
    Per base sequence content (red)
    Per sequence GC content (yellow)
    Overrepresented sequences (yellow)
    Adaptor content (red)
```
<details>
      <summary>2a. Click Here to View FastQC Read 1 Report Image</summary>
  <img width="959" height="362" alt="Read1RawFastQCReport Bc394" src="https://github.com/user-attachments/assets/9138ba46-4ca6-48bd-8384-f314a5f9d791" />

 </details>

```
  Read 2:
    Per base sequence content (red)
    Per sequence GC content (yellow)
    Overrepresented sequences(yellow)
    Adaptor content (red)
```
<details>
      <summary>2b. Click Here to View FastQC Read 2 Report Image</summary>
  <img width="954" height="357" alt="Read2RawFastQCReport Bc394" src="https://github.com/user-attachments/assets/ea9bf506-77d4-4e0b-b5e4-3e1c2a6bc1b0" />
</details>
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
<details>
      <summary>2a. Click Here to View FastQC Read 1 Paired Report Image</summary>
<img width="955" height="369" alt="Read1PairedTrimmedFastQCReport Bc394" src="https://github.com/user-attachments/assets/3907a69a-1565-46d5-8fad-22adab4ed9da" />

</details>

   ```
Read 1 Unpaired:
   Per tile sequence quality (yellow)
   Per base sequence content (red)
   Per sequence GC content (yellow)
   Sequence length distribution (yellow)
```
<details>
      <summary>2b. Click Here to View FastQC Read 1 Unpaired Report Image</summary>
<img width="959" height="364" alt="Read1UnpairedTrimmedFastQCReport Bc394" src="https://github.com/user-attachments/assets/4212e10b-1feb-41ee-8090-bb37a7824a43" />
</details>

  ```
Read 2 Paired:
   Per tile sequence quality (yellow)
   Per base sequence content (red)
   Per sequence GC content (yellow)
   Sequence length distribution (yellow)
   Adapter content (yellow)
```

<details>
      <summary>2c. Click Here to View FastQC Read 2 Paired Report Image</summary>
<img width="956" height="365" alt="Read2PairedTrimmedFastQCReport Bc394" src="https://github.com/user-attachments/assets/dbb00a54-51ed-47ca-a01b-5c1f4209a1c4" />
</details>

   ```
  Read 2 Unpaired:
   Per tile sequence quality (yellow)
   Per base sequence content (red)
   Per sequence GC content (yellow)
   Sequence length distribution (yellow)
   Adapter content (red)
```
<details>
      <summary>2d. Click Here to View FastQC Read 2 Unpaired Report Image</summary>
<img width="959" height="371" alt="Read2UnpairedTrimmedFastQCReport Bc394" src="https://github.com/user-attachments/assets/2d5a8a53-ab3e-4c97-bf73-cef17af6dd96" />
</details>
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

<details>
<summary>Click Here to View Bandage Plot Images</summary>
<img width="308" height="311" alt="Bandange_SpadesGraphNode_Bc394_SNIP" src="https://github.com/user-attachments/assets/255e9900-d552-4959-8686-c9a234c2487f" />

<img width="723" height="431" alt="Bandange_SpadesGraphNode_Bc394" src="https://github.com/user-attachments/assets/d2d19b07-14a2-4660-b51a-c08507a59d4d" />
</details>
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
6. This is the mitochondrial contig file:[Bc394_mitochondrion.csv](https://github.com/user-attachments/files/26286668/Bc394_mitochondrion.csv)

<details>
      <summary>6a. Click Here to View Mitocondrial Contig File Contents</summary>
  
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
</details>
<details>
  <summary>BLASTing Against the B71v2sh Reference Genome</summary>
1. The final Bc394 genome assembly and B71 reference genome were copied from the MCC to the VM using the following command:
  
```
scp /project/farman_s26abt480/xrar222/FinalSubmittedToNCBI/Bc394_final.fasta xrar222@xrar222.cs.uky.edu:/home/xrar222/blast
scp /project/farman_s26abt480/BLAST/B71.fasta xrar222@xrar222.cs.uky.edu:/home/xrar222/blast
```

2. A BLASTn search was performed using the following command to compare the genomes using Bc394 as the query and B71 as the subject.

```
blastn -query Bc394_final.fasta -subject B71.fasta -evalue 1e-100 -outfmt 7 > Bc394.B71.BLAST
```
3. The following code was used to determine how many Bc394 contigs lacked matches in the B71 Reference, which was 334 contigs.

```
grep -c "# 0 hits found" Bc394.B71.BLAST
```

4. A list of the Bc394 contigs with no matches was generated using this code and generating the list below. There were 334 contigs with no matches in the B71 reference genome.

```
awk '/^# Query:/ {q=$3} /^# 0 hits found$/ {print q}' Bc394.B71.BLAST > Bc394Contigs.NoMatch.B71.txt
```
<details>
      <summary>4a. Click Here to View a List of Bc394 Contigs with No Matches</summary>
  
```
Bc394_contig769
Bc394_contig783
Bc394_contig1035~1..4303
Bc394_contig1042
Bc394_contig1049
Bc394_contig1055
Bc394_contig1060
Bc394_contig1099
Bc394_contig1141
Bc394_contig1151
Bc394_contig1158
Bc394_contig1211
Bc394_contig1223
Bc394_contig1272
Bc394_contig1298
Bc394_contig1333
Bc394_contig1350
Bc394_contig1370
Bc394_contig1379
Bc394_contig1382
Bc394_contig1392
Bc394_contig1457
Bc394_contig1461
Bc394_contig1486
Bc394_contig1512
Bc394_contig1520
Bc394_contig1546
Bc394_contig1547
Bc394_contig1550
Bc394_contig1554
Bc394_contig1568
Bc394_contig1577
Bc394_contig1595
Bc394_contig1611
Bc394_contig1616
Bc394_contig1621
Bc394_contig1626
Bc394_contig1638
Bc394_contig1651
Bc394_contig1666
Bc394_contig1667
Bc394_contig1668
Bc394_contig1670
Bc394_contig1699
Bc394_contig1705
Bc394_contig1709
Bc394_contig1728
Bc394_contig1733
Bc394_contig1768
Bc394_contig1770
Bc394_contig1771
Bc394_contig1772~1..536
Bc394_contig1777
Bc394_contig1784
Bc394_contig1785
Bc394_contig1791
Bc394_contig1808
Bc394_contig1821
Bc394_contig1837
Bc394_contig1838
Bc394_contig1840
Bc394_contig1850
Bc394_contig1865
Bc394_contig1866
Bc394_contig1878
Bc394_contig1891
Bc394_contig1898
Bc394_contig1917
Bc394_contig1918
Bc394_contig1920
Bc394_contig1926
Bc394_contig1935
Bc394_contig1944
Bc394_contig1945
Bc394_contig1949
Bc394_contig1953
Bc394_contig1995
Bc394_contig1996
Bc394_contig1998
Bc394_contig2012
Bc394_contig2030
Bc394_contig2039
Bc394_contig2051
Bc394_contig2052
Bc394_contig2088
Bc394_contig2093
Bc394_contig2094
Bc394_contig2096
Bc394_contig2097
Bc394_contig2105
Bc394_contig2119
Bc394_contig2132
Bc394_contig2135
Bc394_contig2157
Bc394_contig2159
Bc394_contig2164
Bc394_contig2166
Bc394_contig2167
Bc394_contig2169
Bc394_contig2170
Bc394_contig2171
Bc394_contig2184
Bc394_contig2189
Bc394_contig2190
Bc394_contig2225
Bc394_contig2228
Bc394_contig2253
Bc394_contig2272
Bc394_contig2279
Bc394_contig2296
Bc394_contig2301
Bc394_contig2302
Bc394_contig2308
Bc394_contig2312
Bc394_contig2321
Bc394_contig2325
Bc394_contig2348
Bc394_contig2349
Bc394_contig2352
Bc394_contig2357
Bc394_contig2363
Bc394_contig2364
Bc394_contig2365
Bc394_contig2368
Bc394_contig2387
Bc394_contig2392
Bc394_contig2401
Bc394_contig2406
Bc394_contig2412
Bc394_contig2415
Bc394_contig2427
Bc394_contig2437
Bc394_contig2439
Bc394_contig2443
Bc394_contig2449
Bc394_contig2467
Bc394_contig2470
Bc394_contig2474
Bc394_contig2476
Bc394_contig2481
Bc394_contig2486
Bc394_contig2488
Bc394_contig2494
Bc394_contig2495
Bc394_contig2503
Bc394_contig2506
Bc394_contig2512
Bc394_contig2513
Bc394_contig2514
Bc394_contig2524
Bc394_contig2527
Bc394_contig2536
Bc394_contig2537
Bc394_contig2548
Bc394_contig2549
Bc394_contig2550
Bc394_contig2551
Bc394_contig2555
Bc394_contig2556
Bc394_contig2560
Bc394_contig2563
Bc394_contig2566
Bc394_contig2567
Bc394_contig2568
Bc394_contig2569
Bc394_contig2570
Bc394_contig2571
Bc394_contig2573
Bc394_contig2576
Bc394_contig2577
Bc394_contig2581
Bc394_contig2582
Bc394_contig2589
Bc394_contig2590
Bc394_contig2591
Bc394_contig2595
Bc394_contig2596
Bc394_contig2599
Bc394_contig2603
Bc394_contig2604
Bc394_contig2605
Bc394_contig2608
Bc394_contig2614
Bc394_contig2615
Bc394_contig2616
Bc394_contig2617
Bc394_contig2618
Bc394_contig2621
Bc394_contig2623
Bc394_contig2626
Bc394_contig2627
Bc394_contig2629
Bc394_contig2631
Bc394_contig2632
Bc394_contig2634
Bc394_contig2636
Bc394_contig2638
Bc394_contig2641
Bc394_contig2643
Bc394_contig2646
Bc394_contig2652
Bc394_contig2653
Bc394_contig2655
Bc394_contig2656
Bc394_contig2661
Bc394_contig2662
Bc394_contig2663
Bc394_contig2672
Bc394_contig2673
Bc394_contig2678
Bc394_contig2680
Bc394_contig2681
Bc394_contig2682
Bc394_contig2686
Bc394_contig2687
Bc394_contig2688
Bc394_contig2690
Bc394_contig2692~26..249
Bc394_contig2693
Bc394_contig2697
Bc394_contig2698
Bc394_contig2699
Bc394_contig2700
Bc394_contig2703
Bc394_contig2704
Bc394_contig2707
Bc394_contig2708
Bc394_contig2713
Bc394_contig2721
Bc394_contig2724
Bc394_contig2725
Bc394_contig2726
Bc394_contig2727
Bc394_contig2729
Bc394_contig2733
Bc394_contig2736
Bc394_contig2742
Bc394_contig2744
Bc394_contig2746
Bc394_contig2747
Bc394_contig2752
Bc394_contig2753
Bc394_contig2755
Bc394_contig2756
Bc394_contig2757
Bc394_contig2758
Bc394_contig2760
Bc394_contig2761
Bc394_contig2765
Bc394_contig2766
Bc394_contig2767
Bc394_contig2769
Bc394_contig2770
Bc394_contig2772
Bc394_contig2773
Bc394_contig2774
Bc394_contig2775
Bc394_contig2776
Bc394_contig2777
Bc394_contig2778
Bc394_contig2779
Bc394_contig2780
Bc394_contig2781
Bc394_contig2782
Bc394_contig2786
Bc394_contig2788
Bc394_contig2789
Bc394_contig2790
Bc394_contig2792
Bc394_contig2794
Bc394_contig2796
Bc394_contig2797
Bc394_contig2800
Bc394_contig2802
Bc394_contig2803
Bc394_contig2807
Bc394_contig2815
Bc394_contig2817
Bc394_contig2818
Bc394_contig2819
Bc394_contig2820
Bc394_contig2822
Bc394_contig2829
Bc394_contig2832
Bc394_contig2834
Bc394_contig2837
Bc394_contig2838
Bc394_contig2839
Bc394_contig2840
Bc394_contig2845
Bc394_contig2846
Bc394_contig2847
Bc394_contig2849
Bc394_contig2850
Bc394_contig2852
Bc394_contig2858
Bc394_contig2860
Bc394_contig2865
Bc394_contig2868
Bc394_contig2869
Bc394_contig2873
Bc394_contig2882
Bc394_contig2883
Bc394_contig2885
Bc394_contig2886
Bc394_contig2887
Bc394_contig2888
Bc394_contig2890
Bc394_contig2893
Bc394_contig2894
Bc394_contig2898
Bc394_contig2899
Bc394_contig2903
Bc394_contig2906
Bc394_contig2907
Bc394_contig2908
Bc394_contig2912
Bc394_contig2921
Bc394_contig2922
Bc394_contig2923
Bc394_contig2925
Bc394_contig2926
Bc394_contig2930
Bc394_contig2934
Bc394_contig2935
Bc394_contig2936
Bc394_contig2937
Bc394_contig2944
Bc394_contig2945
Bc394_contig2949
Bc394_contig2950
Bc394_contig2951
Bc394_contig2954
Bc394_contig2956
```
</details>

5. A new BLAST was run with B71 as the query and Bc394 as the subject to facilitate downstream applications. The following command was used:

```
blastn -query B71.fasta -subject Bc394_final.fasta -evalue 1e-100 -outfmt 7 > B71.Bc394.BLAST
```

</details>
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
  10b . This code also works for verifying gene count on the gff3 file

```
awk '$3 == "gene"' Bc394-maker.gff3 | wc -l
```
  10c. The number of gene found by MAKER in the faste file was verified as 13,098 using this code:

```
grep ">" Bc394.all.maker.proteins.fasta | wc -l
```

</details>
<details>
  <summary>Snap, MAKER, and Augustus Gene Prediction Numbers</summary>
  1.Below is a table summarizing the number of predicted genes across the 3 programs:
  <img width="457" height="159" alt="image" src="https://github.com/user-attachments/assets/bf0a9fdd-8489-4768-a709-3b3f53d10190" />

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

<details>
<summary>3a. A screen shot of IGV browser window showing an example of a gene predicted only by snap</summary>
<img width="515" height="351" alt="GeneSnapOnly" src="https://github.com/user-attachments/assets/a8fbb967-c067-4ce1-ab4c-eeb8dadc37ff" />
</details>

<details>
<summary>3b. A screen shot of IGV browser window showing an example of a gene predicted only by AUGUSTUS</summary>
<img width="845" height="540" alt="AugustusOnlyfinal" src="https://github.com/user-attachments/assets/1beede5b-6f60-4dc5-a50f-949ba2080654" />
</details>

<details>
<summary>3c. A screen shot of IGV browser window showing an example of a gene predicted only by MAKER</summary>
<img width="787" height="470" alt="image" src="https://github.com/user-attachments/assets/552190c6-7c6d-4c40-bb2e-2f5c0e1e65bc" />
</details>

<details>
<summary>3d. A screen shot of a gene where snap and AUGUSTUS predict the same exon/intron structure</summary>
<img width="763" height="334" alt="BothGene" src="https://github.com/user-attachments/assets/93667ae1-6adc-4d52-bf22-b3b413764188" />
</details>

<details>
<summary>3e. Screenshots of genes where snap and AUGUSTUS predict a different exon/intron structure</summary>
<img width="946" height="317" alt="GeneAugustusOnly" src="https://github.com/user-attachments/assets/50bb8a27-d6b9-4ebd-922f-dbcaee47e855" />
<img width="775" height="335" alt="image" src="https://github.com/user-attachments/assets/ea062fae-26fb-4cb7-a45a-cf0ffaf492fd" />
</details>

<details>
<summary>3f. A screen shot where a gene was successfully predicted by snap, AUGUSTUS, and MAKER with external evidence that the prediction is correct</summary>
<img width="601" height="555" alt="image" src="https://github.com/user-attachments/assets/2e430e9d-cb05-4ee3-834e-f22f95069693" />
</details>

4. The Snap gff2 file was not showing introns on IGV, so this code was run to convert the gff2 into a gff3 in the simplest way toallow introns to appear by ordering and labelling the data properly.

```
awk 'BEGIN{OFS="\t";print "##gff-version 3"}$3~/^E(init|xon|term|sngl)$/{
gid=$9;t=gid".t1";
if(!seen[gid]++){gene[gid]=$1 OFS "SNAP" OFS "gene" OFS $4 OFS $5 OFS "." OFS $7 OFS "." OFS "ID="gid;
mrna[gid]=$1 OFS "SNAP" OFS "transcript" OFS $4 OFS $5 OFS "." OFS $7 OFS "." OFS "ID="t";Parent="gid}
cds[t]=cds[t] sprintf("%s\tSNAP\tCDS\t%s\t%s\t%s\t%s\t0\tParent=%s\n",$1,$4,$5,$6,$7,t);
if($4<min[gid]||!min[gid])min[gid]=$4;if($5>max[gid])max[gid]=$5}
END{for(g in gene){sub(/\t[0-9]+\t[0-9]+\t/,"\t"min[g]"\t"max[g]"\t",gene[g]);
sub(/\t[0-9]+\t[0-9]+\t/,"\t"min[g]"\t"max[g]"\t",mrna[g]);
print gene[g];print mrna[g];printf "%s",cds[g".t1"]}}' Bc394-snap.gff2 > Bc394-snap_igv.gff3

```
5. The file was downloaded to my local machine using the following command.

```
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/genes/snap/Bc394-snap_igv.gff3 C:\Users\19042\Downloads\IGVforClass
```

6. The Snap file now shows intronic regions, as shown below:
<details><summary>Click here to see the Snap alignment with introns</summary>
  <img width="533" height="226" alt="SnapShowingIntrons" src="https://github.com/user-attachments/assets/c03847c4-7235-4dd3-8777-53b69055ed4e" />
</details>
   
8. In order to determine if there were regions in the Bc394 genome NOT present in the B71 reference genome by visual comparison in IGV, the BLAST alignment done in step 5 of the BLAST section (B71 as query, Bc394 as subject) was converted to a gff3 using the following code, where Query Seq ID was used as the Seq ID, my source is "Awk" and my Type is "BLAST". Column 9/Gene ID in the gff was "none" and phase was a dot ".".

```
awk 'BEGIN{OFS="\t"; print "##gff-version 3"} $0!~/^#/{st=($7<=$8?"+":"-"); s=($7<=$8?$7:$8); e=($7<=$8?$8:$7); print $1,"Awk","BLAST",s,e,".",st,".","Gene_ID=none"}' B71.Bc394.BLAST > B71.Bc394.BLAST.gff3
```
8. The gff3 file created and the B71 fasta file were downloaded to my local machine using the following commands:

```
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/blast/B71.Bc394.BLAST.gff3 C:\Users\19042\Downloads\IGVforClass
scp xrar222@xrar222.cs.uky.edu:/home/xrar222/blast/B71.fasta C:\Users\19042\Downloads\IGVforClass
```
9. The gff3 file and B71 reference genome were uploaded into IGV to examine the comparison for parts of the B71 genome that are present that the Bc394 genome did not have. You can see the contents of the gff3 in the files menu of this repository under "gff3 B71 vs Bc394".

10. Most chromosomes had small missing chunks of anywhere from 50 bp to 4,000bp. Chromosome 5 was the most complete. Chromosome 8 was the most split up. One of the largest missing chunks was on Chromosome 3, spanning 48,000 bp.
   <details><summary>Click here to see the 48,000bp chunk missing on CHR 3</summary><img width="460" height="206" alt="CHR3Missing" src="https://github.com/user-attachments/assets/f77db2aa-b261-4742-b7df-df46a497ff65" />
</details>


</details>
<details>
<summary>Using RNAseq to Confirm Predictions</summary>
1. HiSat2 was used to align RNAseq from a P. oryzae FR13 and P. oryzae SS1D16 to Bc394. RNASeq reads were transferred to the working directory using the command below. 

  ```
cp /project/farman_s26abt480/RNASeqData/FR13_inCulture.fastq.gz /project/farman_s26abt480/xrar222/RNAseq
cp /project/farman_s26abt480/RNASeqData/SSID116_inPlanta.fastq.gz /project/farman_s26abt480/xrar222/RNAseq

```
2. The script, seen in the second code box below, for the RNASeq mapping process was transferred to the working directory using the following command.

```
cp /project/farman_s26abt480/SLURM_SCRIPTs/hisat2.sh /project/farman_s26abt480/xrar222/RNAseq
```

```
#!/bin/bash

# Aligns RNAseq reads against a specified reference genome

#SBATCH --time 24:00:00
#SBATCH --job-name=hisat2-align
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16
#SBATCH --partition=normal
#SBATCH --mail-type ALL
#SBATCH --mem=128GB
#SBATCH -A cea_farman_s26abt480
#SBATCH --mail-type ALL
#SBATCH --mail-user xrar222@uky.edu

echo "SLURM_NODELIST: "$SLURM_NODELIST
echo "PWD :" $PWD


refpath=$1
refgenome=${refpath/*\//}
refname=${refgenome/_*/}
refname=${refname/\.*/}

RNAseqPath=$2
RNAseqReads=${RNAseqPath/*\//}
RNAseqName=${RNAseqReads/_*/}
RNAseqName=${RNAseqName/\.*/}

# create a directory for the index
mkdir index

# copy refgenome into index directory
cp $refpath index/${refname}.fasta

# build the HISAT2 index
singularity run --app hisat2221 /share/singularity/images/ccs/conda/amd-conda2-centos8.sinf hisat2-build index/${refname}.fasta index/$refname

# create directory for alignments
mkdir ${refname}_alignments


# align reads1 with HISAT2
singularity run --app hisat2221 /share/singularity/images/ccs/conda/amd-conda2-centos8.sinf hisat2 -p 16 \
        -x index/$refname -U $RNAseqPath --no-unal --max-intronlen 2000 --summary-file ${refname}_alignments/${RNAseqName}_${refname}_summary.txt \
        | singularity run --app samtools115 /share/singularity/images/ccs/samtools-libdeflate/samtools-1.15.sinf samtools sort - \
        -@ 16 -O bam -o ${refname}_alignments/${RNAseqName}_${refname}_hits.bam

#index the bam file
singularity run --app samtools115 /share/singularity/images/ccs/samtools-libdeflate/samtools-1.15.sinf samtools index \
        ${refname}_alignments/${RNAseqName}_${refname}_hits.bam
```
3a. The hisat2 script was first run against the Bc394 assembly and the P. oryzae FR13 organism grown in culture. 

```
sbatch hisat2.sh /project/farman_s26abt480/xrar222/FinalSubmittedToNCBI/Bc394_final.fasta FR13_inCulture.fastq.gz
```
3b. The P. oryzae FR13 alignment output was as follows:

```
13977971 reads; of these:
  13977971 (100.00%) were unpaired; of these:
  5898074 (42.20%) aligned 0 times
  7967983 (57.00%) aligned exactly 1 time
  111914 (0.80%) aligned >1 times
  57.80% overall alignment rate
```


4a. The hisat2 script was then run against the Bc394 assembly and the P. oryzae SSID16 grown in plants, specifically rice leaves. The fraction of reads that aligned were:
  
```
sbatch hisat2.sh /project/farman_s26abt480/xrar222/FinalSubmittedToNCBI/Bc394_final.fasta SSID116_inPlanta.fastq.gz
```
4b.  The P. oryzae SSID16 alignment output was as follows:

```
28650911 reads; of these:
  28650911 (100.00%) were unpaired; of these:
  15498672 (54.09%) aligned 0 times
  12967092 (45.26%) aligned exactly 1 time
  185147 (0.65%) aligned >1 times
  45.91% overall alignment rate

```
5. The alignment (.bam) and index (.bai) files were then downloaded to the local machine for viewing in IGV using the following command:

```

scp xrar222@mcc.uky.edu:/project/farman_s26abt480/xrar222/RNAseq/Bc394_alignments/FR13_Bc394_hits.bam C:\Users\19042\Downloads\IGVforClass
scp xrar222@mcc.uky.edu:/project/farman_s26abt480/xrar222/RNAseq/Bc394_alignments/FR13_Bc394_hits.bam.bai C:\Users\19042\Downloads\IGVforClass
scp xrar222@mcc.uky.edu:/project/farman_s26abt480/xrar222/RNAseq/Bc394_alignments/SSID116_Bc394_hits.bam C:\Users\19042\Downloads\IGVforClass
scp xrar222@mcc.uky.edu:/project/farman_s26abt480/xrar222/RNAseq/Bc394_alignments/SSID116_Bc394_hits.bam.bai C:\Users\19042\Downloads\IGVforClass
```

6. The alignments were uploaded into IGV against the Bc394 genome and gene predictions to answer and collect images on the following points of interest:

<details><summary>6a.Genes with predicted introns where the RNAseq data supports the placement of the predicted introns:</summary> IMAGE HERE </details>

<details><summary>6b. Genes with predicted introns where the introns are spliced/not spliced 100% of the time:</summary> IMAGE HERE </details>
  
<details><summary>6c. Genes that are only expressed in culture:</summary> <img width="953" height="547" alt="image" src="https://github.com/user-attachments/assets/5d9de5ba-c552-4b34-9c23-9a26be62c34e" />
 </details>

<details><summary>6d. Genes that are only expressed in planta:</summary> <img width="943" height="553" alt="image" src="https://github.com/user-attachments/assets/cc9ecb36-8ff8-4de6-9d22-04f62f8db2f6" />
 </details>

<details><summary>6e. Predicted genes with no evidence of expression:</summary> <img width="898" height="549" alt="Predicted AlmostNoEvidenceRNA" src="https://github.com/user-attachments/assets/223609d1-b7c9-4831-9562-b3b252d7545c" />
</details>

<details><summary>6f. Expressed genes that were not predicted:</summary> <img width="617" height="554" alt="Expressed NotPredicted" src="https://github.com/user-attachments/assets/2b6675d9-e855-4939-bea8-587bcc5f97e4" />
 </details>

</details>

<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<
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
