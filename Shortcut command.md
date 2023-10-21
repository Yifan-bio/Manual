
This command is to run large scale salmon alignment 
````sh
for i in $(ls ./*/RAW/*.fastq.gz | sed 's/[1-2].fastq.gz//' | uniq); do     
  o=$(basename $i); 
  o=${o//_/}; 
  d=$(echo $i | cut -d "/" -f2) ;
  salmon quant -i ../support_doc/Gencode/salmon_V43_index/salmon_v43_index/ -l A -1 ${i}1.fastq.gz -2 ${i}2.fastq.gz -p 8 --validateMappings --gcBias --seqBias --recoverOrphans -o ./$d/$o ; 
  done
````

