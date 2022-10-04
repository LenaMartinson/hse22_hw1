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

Смотрим на качетсво подрезанных чтений и общую статистику по ним:

```
mkdir fastqc_data_trimmed
fastqc -o fastqc_data_trimmed oil_R1_sample.fastq.trimmed oil_R2_sample.fastq.trimmed oilMP_S4_L001_R1_001_sample.fastq.int_trimmed oilMP_S4_L001_R2_001_sample.fastq.int_trimmed
mkdir multiqc_data_trimmed
multiqc -o multiqc_data_trimmed fastqc_data_trimmed
```

 <image src="/Screenshot from 2022-10-03 22-34-52.png">
 
 <image src="/Screenshot from 2022-10-03 22-35-25.png">
 
 Строим континги из подрезанных чтений:
 
 ```
 platanus assemble -f oil_R1_sample.fastq.trimmed  oil_R2_sample.fastq.trimmed
```
 
  Скаффолды:
  
  ```
  platanus scaffold -c out_contig.fa -IP1 oil_R1_sample.fastq.trimmed oil_R2_sample.fastq.trimmed -OP2 oilMP_S4_L001_R1_001_sample.fastq.int_trimmed oilMP_S4_L001_R2_001_sample.fastq.int_trimmed
  ```
Весь анализ сделан в ноутбуе в колабе: https://colab.research.google.com/drive/18_GLaBxoyPz_L34SCOCaJYQwpIg8-N6b?usp=sharing
  
  Уменьшаем количество гэпов:
  
  ```
  platanus gap_close -c out_scaffold.fa -IP1 oil_R1_sample.fastq.trimmed oil_R2_sample.fastq.trimmed -OP2 oilMP_S4_L001_R1_001_sample.fastq.int_trimmed oilMP_S4_L001_R2_001_sample.fastq.int_trimmed
  ```
  
  Считаем количество гэпов в том же ноутбуке.
  
 Осталось удалить ненужные файлы:
 
 ```
 rf -rm *trimmed
 ```
