# Read_Me Emily MPhil Cholera Project
This file contains the code and background work that I used for my MPhil project at the Wellcome Sanger Institute.\
Date of Project: April-July 2023

Directions for how to use [R Markdown can be found at this Link](https://cambiotraining.github.io/reproducibility-training/rmarkdown.html#R_Markdown)

## Logging Into the Farm
To login from Sanger \
`ssh farm5` \
Enter Password – Ma******!

To login from home \
`Ssh ep16@ssh.sanger.ac.uk` \
Enter Password – ov******U2

## Finding Directories
Three important directories to note 
1.	**The main PaM cholera directory in nfs**\
This is where the cholera data is found/stored.\
Pathway: `/nfs/pathogen/cholera_club/O-antiseq_JVIserogroupRefStrainOantigenLPSloci/`

2.	**Your ep16 directory in lustre**\
This is where you use/copy the cholera data and do your own calculations/computations.\
Pathway: `/lustre/scratch125/pam/teams/team216/ep16/`

3.	**Your ep16 directory in nfs**\
This is where you store your results.\
Pathway: `/nfs/users/nfs_e/ep16`

## Creating a Directory with the Vibrio Cholera Samples 
* Copy the gff directory from nfs into your lustre\
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$ cp -r /nfs/pathogen/cholera_club/O-antiseq_JVIserogroupRefStrainOantigenLPSloci/gff dir.gff_samples`

* Create a file_list with all the samples\
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$ ls /nfs/pathogen/cholera_club/O-antiseq_JVIserogroupRefStrainOantigenLPSloci/gff > file_list.txt`

* Remove the .gff file extension from the sample names in the file_list \
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$sed 's/\.gff$//' file_list.txt > file_list_noext.txt`

* Make list of serogroups to remove from list in excel\
*This was done in an [excel sreadsheet](https://docs.google.com/spreadsheets/d/1RYOf-7zdLE6Z67yM-SPfJGVH4BGBFUrV/edit#gid=1880666571) which contains metadata of the sampled in our dataset. I found the sample IDs of the non-vibrio cholera samples, and added these to a blank page in the text-editor BBEdit. These were then copy-pasted into a text-file in the terminal using nano.* \
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$ nano remove_samples.txt`

* Remove the non-cholera samples from the original file_list to produce a new list of cholera-only samples\
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$ grep -v -f remove_samples.txt file_list_noext.txt > cholera_samples_list.txt`

* Remove '_LPS' from the end of each sample name\
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$ sed -i 's/_LPS//' cholera_samples_list.txt`

* Create a new directory with only the cholera samples\
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$ for i in $(cat cholera_samples_list.txt); do cp /nfs/pathogen/cholera_club/O-antiseq_JVIserogroupRefStrainOantigenLPSloci/gff/${i}.gff dir.cholera_gff_190 ; done`

## Running the Cholera Samples through PANAROO
* Load Modules 
`module load bsub.py/0.42.1`\
`module load panaroo/1.3.2--pyhdfd78af_0`\
`module load ppanggolin/1.2.74--py36h91eb985_0`\

  *Note that you can add these modules to your .bashrc file in your nfs directort so they load aytomatically when you login.* \
  `Cd /nfs/users/nfs_e/ep16`\
  `nano ~/.bashrc`\
  
* Submitting a Job to Panaroo \
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16/dir.cholera_gff_190$ bsub.py 1 job.panarootest_gff_190 panaroo -i /nfs/pathogen/cholera_club/O-antiseq_JVIserogroupRefStrainOantigenLPSloci/gff/*gff -o out.panarootest_gff_190 --clean-mode moderate --merge_paralogs`
Three files are created 
  1. A job file: job.panarootest_gff_190.o \
     This should have a section as the bottom saying 'successfully completed' \
  2. An error file: job.panarootes_gff_190.e \
     This should have no errors indicated \
  4. An output directory: out.panarootest_gff_190 \
    Check these files. Especially the summary statistics to see if perameters need to be changed
    

## Running the Cholera Samples through PPANGGOLIN
* * Submitting a Job to Ppanggolin \
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16/dir.cholera_gff_190$ bsub.py 1 job.ppanggolin1 ppanggolin -i /lustre/scratch125/pam/teams/team216/ep16/dir.training/out.panarootest/gene_presence_absence_roary.csv -o out.ppanggolin --clean-mode moderate --merge_paralogs moderate`

