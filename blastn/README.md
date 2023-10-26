#step1, mkblastdb:
for i in `ls ./`
do
        makeblastdb -in ./$i -dbtype nucl
done

#step2, blastn through the nucleotide database:
for i in `ls ./`
do
	blastn -query all_cds.fna -db ./$i -evalue 1e-5 -perc_identity 80 -qcov_hsp_perc 80 -max_target_seqs 1 -out result/$i.bls.fmt6 -outfmt "6 std gaps qcovs qcovhsp sseq" -num_threads 4
done

#step3, filtering the alignment results through ssANI threshold:
python3 data-filter_ssANI2.py ANI_result.txt 20_genomes_results_all.out 20_genomes_ssANI_output.txt

#step4, architecture search physically clustered genes and sequence extraction:
python3 gene_cluster_extract_final_ssANI5-fragment.py gene_lable 20_genomes_ssANI_output.txt result/final_20_genomes_ssANI_output_statics
