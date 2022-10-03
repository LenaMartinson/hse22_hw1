# hse22_hw1

Делаем папку для домашнего задания и симлинки:

```
mkdir hw1
cd hw1
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq
```

Дальше случайно выбираем нужное количество чтений:

```
seqtk sample -s 816 oil_R1.fastq 5000000 > oil_R1_sample.fastq
seqtk sample -s 816 oil_R2.fastq 5000000 > oil_R2_sample.fastq
seqtk sample -s 816 oilMP_S4_L001_R1_001.fastq 1500000 > oilMP_S4_L001_R1_001_sample.fastq 
seqtk sample -s 816 oilMP_S4_L001_R2_001.fastq 1500000 > oilMP_S4_L001_R2_001_sample.fastq 
```

Оцениваем качество исходных чтений с помощью fastQC:

```
mkdir fastqc_data 
fastqc -o fastqc_data oil_R1.fastq oil_R2.fastq oilMP_S4_L001_R2_001.fastq oilMP_S4_L001_R1_001.fastq
```

 Получаем статистику с помощью multiQC:
 
 ```
 mkdir multiqc_data
 multiqc -o multiqc_data fastqc_data
 ```
