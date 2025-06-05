## Pre-processing


#kinship matrix is produced using the emmax-kin command from the emmax package:

```plink2 --vcf NAM_v5_PHGv1_all_chroms_vcftools_wanted.vcf --max-alleles 2 --make-bed --out NAM_v5_PHGv1_all_chroms_vcftools_wanted```

Assign family ID

```
cat NAM_v5_PHGv1_all_chroms_vcftools_wanted.fam | awk '{print($2" "$2" "$3" "$4" "$5" "$6)}' > temp.fam
mv temp.fam NAM_v5_PHGv1_all_chroms_vcftools_wanted.fam
```

Assign SNP ID
```
cat NAM_v5_PHGv1_all_chroms_vcftools_wanted.bim | awk '{print($1"\t"$1"_"$4"\t"$3"\t"$4"\t"$5"\t"$6)}'  > temp.header.bim
mv temp.header.bim NAM_v5_PHGv1_all_chroms_vcftools_wanted.bim
```


```plink --bfile NAM_v5_PHGv1_all_chroms_vcftools_wanted --recode12 --output-missing-genotype 0 --transpose --out snp```

```emmax-kin -v -h -d 10 snp```





Multiple sequence alignment or multiple structure alignment and similarity matrix for each pan-gene (python)  
   MAFFT ```mafft --maxiterate 10000 --localpair ./100/sequences > ./100/sequences.mafft ```  
   Muscle ```muscle -in ./100/sequences > ./100/sequences.muscle ```  
   T-coffee ```t_coffee  ./100/sequences -output fasta_aln -thread 10 -outfile ./100/sequences.t_coffee.fast_aln ```  
   US-align ```USalign -dir ./100/ ./100/list -suffix .pdb -mm 4 -mol prot -o ./100/sugary1 > ./100/alignment ```  

Similarity matrix for multiple sequence alignment is computed by BLOSUM62 score. e.g. mafftToSequenceSimilarity.py   
Similarity matrix for multiple structure alignment is computed by TM score. usalignMatrix.py
 
     
## Processing PWAS-data_processing.R

1. Projection: "mergedPanGeneTables" is projection result (pangene * individual) using PHG, the procedure will acqurie protein name of offspring according to NAM founder lines.   

2. Quality control was processed using the following steps: The similarity matrix for each pan gene covered all individual protein variants and was present in the IBD matrix. Secondly, the sum of variance after centering was greater than 1/25 for at least one metric. The variance explained by each principal component for each kept pan-gene group was greater than 4*10-4.

4. Pan-gene group matrix and PCs.

5. Kinship matrix.

## Association PWAS-association_testing-HQK.R
