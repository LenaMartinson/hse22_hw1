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
 
 <image src="/Screenshot from 2022-10-03 22-02-11.png">
 
 <image src="/Screenshot from 2022-10-03 22-06-39.png">
 
 Подрезаем чтения:
 
 ```
 platanus_trim oil_R1_sample.fastq oil_R2_sample.fastq
 platanus_internal_trim oilMP_S4_L001_R2_001_sample.fastq oilMP_S4_L001_R1_001_sample.fastq
 ```
 
 Удаляем исходный файлы:
 
 ```
rm -rf oil_R1_sample.fastq
rm -rf oil_R2_sample.fastq
rm -rf oilMP_S4_L001_R1_001_sample.fastq 
rm -rf oilMP_S4_L001_R2_001_sample.fastq 
```
