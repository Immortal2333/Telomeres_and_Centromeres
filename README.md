# To find telomeres and centromeres
This is a method to find some significant information of `genome.fa`, which include the locations, lengths, minimum repeat units, etc. of telomeres and centromeres. Here, we use the genome of grape T2T as an example to show how we find them quickly and accurately.

![workflow](man/figures/README-geom_gene_arrow-1.png)<!-- -->

## Telomeres
### Install [TIDK](https://github.com/tolkit/telomeric-identifier)
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

## Centromeres
Please download two tools, [EDTA](https://github.com/oushujun/EDTA) and [TRF](https://github.com/Adamtaranto/TRF2GFF), before you start searching.
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
#### Grap keywords
As reported, centromeres are high relating to TE (LTR_Copia, LTR_Gypsy, etc.) and some specific centromeric tandem repeat units. Therefore, we should extract keywords, such as Copia, Gypsy, Helitron, and etc., from `genome.mod.EDTA.TEanno.gff3` respectively, which can attain different type of TE (format `.gff3`).
```
grep 'Copia' genome.mod.EDTA.TEanno.gff3 > TE_Copia.split.gff3
grep 'Gypsy' genome.mod.EDTA.TEanno.gff3 > TE_Gypsy.split.gff3
grep 'Helitron' genome.mod.EDTA.TEanno.gff3 > TE_Helitron.split.gff3
grep 'MULE-MuDR' genome.mod.EDTA.TEanno.gff3 > TE_MULE-MuDR.split.gff3
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
You can find `TRF2GFF.py` in [TRF2GFF](https://github.com/Adamtaranto/TRF2GFF). \
\
The output of `genome_trf.split.txt` is as follows.
```
PN01   2       668     ID=TRF_00001    period=7        copies=94.9     consensus_size=7        cons_seq=AAGTTTA
PN01   592     699     ID=TRF_00002    period=7        copies=14.6     consensus_size=7        cons_seq=GTTTCAC ...
```
\
Additionally, we can use EXCEL to open `genome_trf.split.txt` and then screen the top five of repeat times of `period` in each chromesome (filtered condition: period >= 30, copies >= 2.0. [Melters et al., 2013](https://genomebiology.biomedcentral.com/articles/10.1186/gb-2013-14-1-r10)). And then to extract these repeat units from `genome_trf.gff3` respectively.
```
grep 'period=107' genome_trf.gff3 > trf_107bp.split.gff3
grep 'period=214' genome_trf.gff3 > trf_214bp.split.gff3
grep 'period=428' genome_trf.gff3 > trf_428bp.split.gff3 ...
```

### Find centromeres by using IGV
Please download IGV from [offical website](https://software.broadinstitute.org/software/igv/download) first.\
\
Please perpare four type of files: `genome.fa`, `genome.gff3`,  `TE_XXX.split.gff3`, and `trf_XXXbp.split.gff3`. And then put them into IGV to visual your data. You can zoom in and out and divide core centromeric region according to the density of genome annotation, TE and TRF as follows.\
\
You can see the low frequency peak of `genome.gff3` and `TE_XXX.split.gff3` in centromeric regions, while there is the high frequency peak of (top five) `trf_XXXbp.split.gff3`.\
![IGV_all](man/figures/README-geom_gene_arrow-1.png)<!-- -->
In grape, you can find the top five of repeat units is 107 and its times, such as 214bp, 321bp, 428bp, and etc., in most of chromesome. \
![IGV_chr01](man/figures/README-geom_gene_arrow-1.png)<!-- -->

However, there have some different patterns in chr03, chr18. It can be 135bp and 66bp (and its times). \
![IGV_chr03](man/figures/README-geom_gene_arrow-1.png)<!-- -->
![IGV_chr18](man/figures/README-geom_gene_arrow-1.png)<!-- -->

## Citations
Xiaoya Shi, et al. The complete reference genome for grapevine (Vitis vinifera L.) genetics and breeding. (2022)\

## Contact
Xu Wang (571720850@qq.com)

