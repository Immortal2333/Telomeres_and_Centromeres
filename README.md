# To find telomeres and centromeres
This is a method to find some significant information of `genome.fa`, which include the location, length, minimum repeat unit, etc. of telomeres and centromeres. Here, we use the genome of grape T2T as an example to show how we find them quickly and accurately. We also prepare `a test file` that you can find above. 

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
Reference website: http://telomerase.asu.edu/sequences_telomere.html

### Search specific repeat and plot
```
tidk search -f genome.fa -s TTTAGGG -o ./genome.search --dir /your/path/telomere_find

tidk plot -c /your/path/telomere_find/genome.search_telomeric_repeat_windows.csv -o ./genome.search
```
You can both check the image file `.svg` visually and the original file `.csv` to locate the telomeres. \
If you want to explore more fuctions of TIDK, please click this link: https://github.com/tolkit/telomeric-identifier

## Centromeres
