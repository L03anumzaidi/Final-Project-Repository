# Final-Project-Repository

Lab 5

ncbi-acc-download -F fasta -m protein XP_001639017.2
#downloading the protein data from ncbi genbank and outputting a fasta/.fas file

blastp -db allprotein.fas -query XP_001639017.2.fa -outfmt 0 -max_hsps 1 -out nema.blastp.typical.out
 
blastp -db allprotein.fas -query XP_001639017.2.fa -outfmt "6 sseqid pident length mismatch gapopen evalue bitscore pident stitle"  -max_hsps 1 -out nema.blastp.detail.out

awk '{if ($6<0.00000000000001)print $1 }' nema.blastp.detail.out > nema.blastp.detail.filtered.out

wc -l nema.blastp.detail.filtered.out

seqkit grep --pattern-file nema.blastp.detail.filtered.out allprotein.fas > nema.blastp.detail.filtered.fas

muscle -in nema.blastp.detail.filtered.fas -out nema.blastp.detail.filtered.aligned.fas

t_coffee -other_pg seq_reformat -in nema.blastp.detail.filtered.aligned.fas -output sim

alv -k nema.blastp.detail.filtered.aligned.fas | less -r 

alv -kli --majority nema.blastp.detail.filtered.aligned.fas | less -RS

t_coffee -other_pg seq_reformat -in nema.blastp.detail.filtered.aligned.fas -action +rm_gap 50 -out nema.blastp.detail.filtered.aligned.r50.fas

alv -kli --majority nema.blastp.detail.filtered.aligned.r50.fas | less -RS
