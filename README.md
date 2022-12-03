# To find telomeres and centromeres
This is a method to find some significant information of `genome.fa`, which include the locations, lengths, minimum repeat units, etc. of telomeres and centromeres. Here, we use the genome of grape T2T as an example to show how we find them quickly and accurately. We also prepare `a test file` that you can find above. 

## Telomeres
### Install TIDK
```
conda install -c bioconda tidk
```
Here, you can check this script whether working correctly by `tidk -h`

### Explore telomere repeat units
```
tidk explore -f genome.fa --minimum 5 --maximum 12 -o ./genome.explore -t 10 --log --dir /your/path/telomere_find --extension tsv
```
Actually, TTTAGGG/CCCTAAA is a common repeat of telomeres in most plants. \
Reference website: http://telomerase.asu.edu/sequences_telomere.html.

### Search specific repeat and plot
```
tidk search -f genome.fa -s TTTAGGG -o ./genome.search --dir /your/path/telomere_find

tidk plot -c /your/path/telomere_find/genome.search_telomeric_repeat_windows.csv -o ./genome.search
```
You can both check the image file `.svg` visually and the original file `.csv` to locate the telomeres. \
If you want to explore more functions of TIDK, please click this link: https://github.com/tolkit/telomeric-identifier.

## Centromeres
Please download two tools, EDTA (https://github.com/oushujun/EDTA) and TRF (https://github.com/Adamtaranto/TRF2GFF), before you start searching.
### EDTA
```
# Install
git clone https://github.com/oushujun/EDTA.git
cd EDTA
conda env create -f EDTA.yml
conda activate EDTA
perl EDTA.pl

# RUN
source /your/path/anaconda3/bin/activate EDTA

perl EDTA.pl --genome genome.fa --sensitive 1 --overwrite 1 --anno 1 --species others --threads 10
```
### TRF
```
# Install
conda create -n TRF
conda activate TRF
conda install -c bioconda trf

# RUN (defualt)
trf genome.fa 2 5 7 80 10 50 2000

python TRF2GFF.py -d trf_output.dat -o genome_trf.gff3

# Filter redundant information
cat genome_trf.gff3 | awk '{split($9,a,";");print $1"\t"$4"\t"$5"\t"a[1]"\t"a[2]"\t"a[3]"\t"a[4]"\t"a[9]}' > genome_trf.split.txt
```
You can find `TRF2GFF.py` in https://github.com/Adamtaranto/TRF2GFF. \
\
The output of `genome_trf.split.txt` is as follows.
```
PN01   2       668     ID=TRF_00001    period=7        copies=94.9     consensus_size=7        cons_seq=AAGTTTA
PN01   592     699     ID=TRF_00002    period=7        copies=14.6     consensus_size=7        cons_seq=GTTTCAC ...
```
### Grap keywords
As reported, centromeres are high relating to TE( LTR_Copia, LTR_Gypsy, etc.) and some specific centromeric tandem repeat units. Therefore, we should 


### Find centromeres by using IGV
Please download IGV from https://software.broadinstitute.org/software/igv/download.\
\
Please perpare three files: `genome.gff`, `genome.mod.EDTA.TEanno.gff3`, and `genome_trf.split.txt`.

