# Data download

http://www.ebi.ac.uk/ena/data/view/SAMN03083651

# Trimming with scythe

Began by trimming the data with scythe (0.7_scythe_qsub.pl):

```
#!/usr/bin/perl
# This script will use scythe to trim 3' adapter seqs

my $status;
my @files;

@files = glob("../rhesus_macaque_genomez/*/*.fastq.gz");

# Take off the ".fastq.gz" suffix
foreach(@files){
    $_=substr($_, 0, -9);
}

my @temp;
my $jobname;
foreach(@files){
    @temp=split("\/",$_);
    $jobname=$temp[3];
    print $jobname,"\n";
    my $commandline = "qsub -cwd -b y -N ".$jobname."_qsub bash -c \"../bin/scythe/scythe -a ../bin/scythe/adap_TruSeqonly.fa -p 0.1 -q sanger ".$_.".fastq.gz | gzip
 > ".$_."scythe.fastq.gz\"";
    # the -cwd means run from the current working directory
    # the -b y means we are going to execute a binary file
    # the bash -c option lets the command be specified in quotes in bash format - very useful
    print $commandline,"\n\n";
    $status = system($commandline);
}
```
