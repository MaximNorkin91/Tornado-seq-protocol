# Tornado-seq-protocol
# Custom code for the analysis of targeted RNA-seq raw data

# Each individual unique fastq.gz file located in "folder_N" and was processed with the following script: 

cd folder_N/
umi_tools extract --stdin=Lib_1.fastq.gz --bc-pattern=NNNNNNNNNN --log=processed.log --stdout processed.fastq
bowtie --threads 4 -v 2 -m 10 -a Custom_genome processed.fastq --sam > processed.sam
samtools import TORNADO-seq_206_genes.fa processed.sam processed.bam
samtools sort processed.bam -o exampleprocessed.bam
samtools index exampleprocessed.bam
umi_tools dedup -I exampleprocessed.bam --output-stats=deduplicated -S deduplicatedprocessed.bam
samtools view -h -o deduplicatedprocessed.sam exampleprocessed.bam
sort -k 3,3 -k 4,4n deduplicatedprocessed.sam > processed.sam.sorted
cufflinks --no-length-correction -g TORNADO-seq_206_genes.GFF ./processed.sam.sorted
cp transcripts.gtf transcripts.txt
cd ..





