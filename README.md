# MPhil Cholera Project
This file contains the code and background work that I used for my MPhil project at the Wellcome Sanger Institute

Directions for how to use [R Markdown can be found at this Link](https://cambiotraining.github.io/reproducibility-training/rmarkdown.html#R_Markdown)

## Finding directories
Three important directories to know 
1.	**The main PaM cholera directory in nfs**\
This is where the cholera data is found/stored.\
Pathway: `/nfs/pathogen/cholera_club/O-antiseq_JVIserogroupRefStrainOantigenLPSloci/`

2.	**Your ep16 directory in lustre**\
This is where you use/copy the cholera data and do your own calculations/computations.\
Pathway: `/lustre/scratch125/pam/teams/team216/ep16/`

3.	**Your ep16 directory in nfs**\
This is where you store your results.\
Pathway: `/nfs/users/nfs_e/ep16`

## Creating a directory with the Vibrio Cholera Samples 
* Copy the gff directory from nfs into your lustre\
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$ cp -r /nfs/pathogen/cholera_club/O-antiseq_JVIserogroupRefStrainOantigenLPSloci/gff dir.gff_samples`

* Create a file_list with all the samples\
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$ ls /nfs/pathogen/cholera_club/O-antiseq_JVIserogroupRefStrainOantigenLPSloci/gff > file_list.txt`

* Remove the .gff file extension from the sample names in the file_list \
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$sed 's/\.gff$//' file_list.txt > file_list_noext.txt`

* Make list of serogroups to remove from list in excel\
This was done in an [excel sreadsheet](https://docs.google.com/spreadsheets/d/1RYOf-7zdLE6Z67yM-SPfJGVH4BGBFUrV/edit#gid=1880666571) which contains metadata of the sampled in our dataset. I found the sample IDs of the non-vibrio cholera samples, and added these to a black page in the text-editor BBEdit. These were then copy-pasted into a text-file in the terminal using nano. \
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$ nano remove_samples.txt`

* Remove the non-cholera samples from the original file_list to produce a new list of cholera-only samples\
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$ grep -v -f remove_samples.txt file_list_noext.txt > cholera_samples_list.txt`

* Remove '_LPS' from the end of each sample name\
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$ sed -i 's/_LPS//' cholera_samples_list.txt`

* Create a new directory with only the cholera samples\
`ep16@farm5-head1:/lustre/scratch125/pam/teams/team216/ep16$/dir.gff_samples$ for i in $(cat cholera_samples_list.txt); do cp /nfs/pathogen/cholera_club/O-antiseq_JVIserogroupRefStrainOantigenLPSloci/gff/${i}.gff dir.new_cholera ; done`
