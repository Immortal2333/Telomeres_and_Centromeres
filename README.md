# To find telomeres and centromeres
This is a method to find some significant information of `genome.fa`, which include the location, length, minimum repeat unit, etc. of telomeres and centromeres. Here, we use the genome of grape T2T as a example to show how we find them quickly and accurately. We also prepare `a test file` that you can find above. 

## Telemeres
### Install TIDK
```
conda install -c bioconda tidk
```
Here, you can check this script whether working correctly by `tidk -h`

### Explore telemere repeat units
```
tidk explore -f genome.fa --minimum 5 --maximum 12 -o ./genome.explore -t 10 --log --dir /your/path/telomere_find --extension tsv
```
Actually, TTTAGGG/CCCTAAA is common repeat of telemeres in most plant
Reference website: http://telomerase.asu.edu/sequences_telomere.html



## Centromeres
