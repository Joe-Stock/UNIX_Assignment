# Start by getting to know the files that I am working with #

### The first file is fang_et_al_genotypes.txt ###

wc fang_et_al_genotypes.txt

### Word Count returns there are 2783/2744038/11054722 lines/words/bytes in the file. ###

head -1 fang_et_al_genotypes.txt | wc -w

### Returns there are 986 words/columns in fang_et_al_genotypes. ###

### The second file is snp_position.txt. ###

wc snp_position.txt

### Word count returns there are 984/13198/83747 lines/words/bytes in the file. ###

head -1 snp_position.txt | wc -w

### Returns there are 15 words/columns in snp_position. ###

### While these numbers are informative, they don't fully get to working with the data. ###
### To better understand the data I'll need to look at a portion of the data, as these files are too large to look at by themselves. ###

head -2 fang_et_al_genotypes.txt
head -2 snp_position.txt

### This returns the first two lines from the txt file. ###

### Investigating these files informs me that the 4th - 986th columns in fang_et_al_genotypes correspond identically to the 2nd - 984th rows in snp_position. ###

### I'm also curious how many unique values of the column Group exist within the fang_et_al_genotypes file. ###

cat fang_et_al_genotypes.txt | cut -f 3 | sort | uniq

### Returns all unique values for the column Group. There are 16 unique values, however, I'm only interested in working with 6, 3 for maize and 3 for teosinte. ###

### I need to cut down the fang_et_al_genotypes file into a maize only file and a teosinte only file. But I want to keep the header also. ###

head -1 fang_et_al_genotypes.txt > head_fang.txt
grep 'ZMMIL\|ZMMLR\|ZMMMR' fang_et_al_genotypes.txt > maize_genotypes.txt
grep 'ZMPBA\|ZMPIL\|ZMPJA' fang_et_al_genotypes.txt > teosinte_genotypes.txt

cat head_fang.txt maize_genotypes.txt > maize_genotypes_head.txt
cat head_fang.txt teosinte_genotypes.txt > teosinte_genotypes_head.txt

### This tool cuts down the full genotype file into more manageable files for only maize and only teosinte. ###

### But now I want to go one step further. I only need a portion of the data and I want to join the files together to create a new master file. ###

### From the snp_position file I only want to keep the 1st, 3rd and 4th columns. ###
awk '{print $1,$3,$4}' snp_position.txt > edit_snp_position.txt

### From the two genotype files I want to remove the first 3 columns, so that when I join the two files they will have matching lines. ###

cut -f4-986 maize_genotypes_head.txt > edit_maize_genotypes.txt
cut -f4-986 teosinte_genotypes_head.txt > edit_teosinte_genotypes.txt

### Now that I have raw genotype files I need to transpose to be able to join the two files. ###

awk -f transpose.awk edit_maize_genotypes.txt > transposed_maize_genotypes.txt
awk -f transpose.awk edit_teosinte_genotypes.txt > transposed_teosinte_genotypes.txt

### The last thing I need to do before I join the files is remove the header from the edit_snp_position file. ###

tail -n+2 edit_snp_position.txt > edit2_snp_position.txt

### I want to now check total lines for each of the editted files I have. ###

wc -l edit2_snp_position.txt
wc -l tranposed_maize_genotypes.txt
wc -l transposed_teosinte_genotypes.txt

### All files have 983 lines of data. I am ready to join the files. ###

join edit2_snp_position.txt transposed_maize_genotypes.txt > maize_joined.txt
join edit2_snp_position.txt transposed_teosinte_genotypes.txt > teosinte_joined.txt

### Now it is time to separate out chromosomes 1 - 10 into their own files and sort in increasing position and decreasing position. Also, I will separate out files that consist of unknown and multiple position. ###

old='?/?'
new='?'
new2='-'

### First I will separate the maize files. ###

awk '(NR>0) && ($2 == 1) && ($3 != "multiple")' maize_joined.txt > maize_1_increase.txt | sed -i "s%$old%$new%g" maize_1_increase.txt | sort -n -k 3 maize_1_increase.txt -o maize_1_increase.txt

awk '(NR>0) && ($2 == 2) && ($3 != "multiple")' maize_joined.txt > maize_2_increase.txt | sed -i "s%$old%$new%g" maize_2_increase.txt | sort -n -k 3 maize_2_increase.txt -o maize_2_increase.txt

awk '(NR>0) && ($2 == 3) && ($3 != "multiple")' maize_joined.txt > maize_3_increase.txt | sed -i "s%$old%$new%g" maize_3_increase.txt | sort -n -k 3 maize_3_increase.txt -o maize_3_increase.txt

awk '(NR>0) && ($2 == 4) && ($3 != "multiple")' maize_joined.txt > maize_4_increase.txt | sed -i "s%$old%$new%g" maize_4_increase.txt | sort -n -k 3 maize_4_increase.txt -o maize_4_increase.txt

awk '(NR>0) && ($2 == 5) && ($3 != "multiple")' maize_joined.txt > maize_5_increase.txt | sed -i "s%$old%$new%g" maize_5_increase.txt | sort -n -k 3 maize_5_increase.txt -o maize_5_increase.txt

awk '(NR>0) && ($2 == 6) && ($3 != "multiple")' maize_joined.txt > maize_6_increase.txt | sed -i "s%$old%$new%g" maize_6_increase.txt | sort -n -k 3 maize_6_increase.txt -o maize_6_increase.txt

awk '(NR>0) && ($2 == 7) && ($3 != "multiple")' maize_joined.txt > maize_7_increase.txt | sed -i "s%$old%$new%g" maize_7_increase.txt | sort -n -k 3 maize_7_increase.txt -o maize_7_increase.txt

awk '(NR>0) && ($2 == 8) && ($3 != "multiple")' maize_joined.txt > maize_8_increase.txt | sed -i "s%$old%$new%g" maize_8_increase.txt | sort -n -k 3 maize_8_increase.txt -o maize_8_increase.txt

awk '(NR>0) && ($2 == 9) && ($3 != "multiple")' maize_joined.txt > maize_9_increase.txt | sed -i "s%$old%$new%g" maize_9_increase.txt | sort -n -k 3 maize_9_increase.txt -o maize_9_increase.txt

awk '(NR>0) && ($2 == 10) && ($3 != "multiple")' maize_joined.txt > maize_10_increase.txt | sed -i "s%$old%$new%g" maize_10_increase.txt | sort -n -k 3 maize_10_increase.txt -o maize_10_increase.txt

awk '(NR>0) && ($2 == 1) && ($3 != "multiple")' maize_joined.txt > maize_1_decrease.txt | sed -i "s%$old%$new2%g" maize_1_decrease.txt | sort -n -r -k 3 maize_1_decrease.txt -o maize_1_decrease.txt

awk '(NR>0) && ($2 == 2) && ($3 != "multiple")' maize_joined.txt > maize_2_decrease.txt | sed -i "s%$old%$new2%g" maize_2_decrease.txt | sort -n -r -k 3 maize_2_decrease.txt -o maize_2_decrease.txt

awk '(NR>0) && ($2 == 3) && ($3 != "multiple")' maize_joined.txt > maize_3_decrease.txt | sed -i "s%$old%$new2%g" maize_3_decrease.txt | sort -n -r -k 3 maize_3_decrease.txt -o maize_3_decrease.txt

awk '(NR>0) && ($2 == 4) && ($3 != "multiple")' maize_joined.txt > maize_4_decrease.txt | sed -i "s%$old%$new2%g" maize_4_decrease.txt | sort -n -r -k 3 maize_4_decrease.txt -o maize_4_decrease.txt

awk '(NR>0) && ($2 == 5) && ($3 != "multiple")' maize_joined.txt > maize_5_decrease.txt | sed -i "s%$old%$new2%g" maize_5_decrease.txt | sort -n -r -k 3 maize_5_decrease.txt -o maize_5_decrease.txt

awk '(NR>0) && ($2 == 6) && ($3 != "multiple")' maize_joined.txt > maize_6_decrease.txt | sed -i "s%$old%$new2%g" maize_6_decrease.txt | sort -n -r -k 3 maize_6_decrease.txt -o maize_6_decrease.txt

awk '(NR>0) && ($2 == 7) && ($3 != "multiple")' maize_joined.txt > maize_7_decrease.txt | sed -i "s%$old%$new2%g" maize_7_decrease.txt | sort -n -r -k 3 maize_7_decrease.txt -o maize_7_decrease.txt

awk '(NR>0) && ($2 == 8) && ($3 != "multiple")' maize_joined.txt > maize_8_decrease.txt | sed -i "s%$old%$new2%g" maize_8_decrease.txt | sort -n -r -k 3 maize_8_decrease.txt -o maize_8_decrease.txt

awk '(NR>0) && ($2 == 9) && ($3 != "multiple")' maize_joined.txt > maize_9_decrease.txt | sed -i "s%$old%$new2%g" maize_9_decrease.txt | sort -n -r -k 3 maize_9_decrease.txt -o maize_9_decrease.txt

awk '(NR>0) && ($2 == 10) && ($3 != "multiple")' maize_joined.txt > maize_10_decrease.txt | sed -i "s%$old%$new2%g" maize_10_decrease.txt | sort -n -r -k 3 maize_10_decrease.txt -o maize_10_decrease.txt

awk '(NR>0) && ($2 == "unknown")' maize_joined.txt > maize_unknown.txt

awk '(NR>0) && ($2 == "multiple") || ($3 == "multiple")' maize_joined.txt > maize_multiple.txt

### Now I will separate the teosinte files. ###

awk '(NR>0) && ($2 == 1) && ($3 != "multiple")' teosinte_joined.txt > teosinte_1_increase.txt | sed -i "s%$old%$new%g" teosinte_1_increase.txt | sort -n -k 3 teosinte_1_increase.txt -o teosinte_1_increase.txt

awk '(NR>0) && ($2 == 2) && ($3 != "multiple")' teosinte_joined.txt > teosinte_2_increase.txt | sed -i "s%$old%$new%g" teosinte_2_increase.txt | sort -n -k 3 teosinte_2_increase.txt -o teosinte_2_increase.txt

awk '(NR>0) && ($2 == 3) && ($3 != "multiple")' teosinte_joined.txt > teosinte_3_increase.txt | sed -i "s%$old%$new%g" teosinte_3_increase.txt | sort -n -k 3 teosinte_3_increase.txt -o teosinte_3_increase.txt

awk '(NR>0) && ($2 == 4) && ($3 != "multiple")' teosinte_joined.txt > teosinte_4_increase.txt | sed -i "s%$old%$new%g" teosinte_4_increase.txt | sort -n -k 3 teosinte_4_increase.txt -o teosinte_4_increase.txt

awk '(NR>0) && ($2 == 5) && ($3 != "multiple")' teosinte_joined.txt > teosinte_5_increase.txt | sed -i "s%$old%$new%g" teosinte_5_increase.txt | sort -n -k 3 teosinte_5_increase.txt -o teosinte_5_increase.txt

awk '(NR>0) && ($2 == 6) && ($3 != "multiple")' teosinte_joined.txt > teosinte_6_increase.txt | sed -i "s%$old%$new%g" teosinte_6_increase.txt | sort -n -k 3 teosinte_6_increase.txt -o teosinte_6_increase.txt

awk '(NR>0) && ($2 == 7) && ($3 != "multiple")' teosinte_joined.txt > teosinte_7_increase.txt | sed -i "s%$old%$new%g" teosinte_7_increase.txt | sort -n -k 3 teosinte_7_increase.txt -o teosinte_7_increase.txt

awk '(NR>0) && ($2 == 8) && ($3 != "multiple")' teosinte_joined.txt > teosinte_8_increase.txt | sed -i "s%$old%$new%g" teosinte_8_increase.txt | sort -n -k 3 teosinte_8_increase.txt -o teosinte_8_increase.txt

awk '(NR>0) && ($2 == 9) && ($3 != "multiple")' teosinte_joined.txt > teosinte_9_increase.txt | sed -i "s%$old%$new%g" teosinte_9_increase.txt | sort -n -k 3 teosinte_9_increase.txt -o teosinte_9_increase.txt

awk '(NR>0) && ($2 == 10) && ($3 != "multiple")' teosinte_joined.txt > teosinte_10_increase.txt | sed -i "s%$old%$new%g" teosinte_10_increase.txt | sort -n -k 3 teosinte_10_increase.txt -o teosinte_10_increase.txt

awk '(NR>0) && ($2 == 1) && ($3 != "multiple")' teosinte_joined.txt > teosinte_1_decrease.txt | sed -i "s%$old%$new2%g" teosinte_1_decrease.txt | sort -n -r -k 3 teosinte_1_decrease.txt -o teosinte_1_decrease.txt

awk '(NR>0) && ($2 == 2) && ($3 != "multiple")' teosinte_joined.txt > teosinte_2_decrease.txt | sed -i "s%$old%$new2%g" teosinte_2_decrease.txt | sort -n -r -k 3 teosinte_2_decrease.txt -o teosinte_2_decrease.txt

awk '(NR>0) && ($2 == 3) && ($3 != "multiple")' teosinte_joined.txt > teosinte_3_decrease.txt | sed -i "s%$old%$new2%g" teosinte_3_decrease.txt | sort -n -r -k 3 teosinte_3_decrease.txt -o teosinte_3_decrease.txt

awk '(NR>0) && ($2 == 4) && ($3 != "multiple")' teosinte_joined.txt > teosinte_4_decrease.txt | sed -i "s%$old%$new2%g" teosinte_4_decrease.txt | sort -n -r -k 3 teosinte_4_decrease.txt -o teosinte_4_decrease.txt

awk '(NR>0) && ($2 == 5) && ($3 != "multiple")' teosinte_joined.txt > teosinte_5_decrease.txt | sed -i "s%$old%$new2%g" teosinte_5_decrease.txt | sort -n -r -k 3 teosinte_5_decrease.txt -o teosinte_5_decrease.txt

awk '(NR>0) && ($2 == 6) && ($3 != "multiple")' teosinte_joined.txt > teosinte_6_decrease.txt | sed -i "s%$old%$new2%g" teosinte_6_decrease.txt | sort -n -r -k 3 teosinte_6_decrease.txt -o teosinte_6_decrease.txt

awk '(NR>0) && ($2 == 7) && ($3 != "multiple")' teosinte_joined.txt > teosinte_7_decrease.txt | sed -i "s%$old%$new2%g" teosinte_7_decrease.txt | sort -n -r -k 3 teosinte_7_decrease.txt -o teosinte_7_decrease.txt

awk '(NR>0) && ($2 == 8) && ($3 != "multiple")' teosinte_joined.txt > teosinte_8_decrease.txt | sed -i "s%$old%$new2%g" teosinte_8_decrease.txt | sort -n -r -k 3 teosinte_8_decrease.txt -o teosinte_8_decrease.txt

awk '(NR>0) && ($2 == 9) && ($3 != "multiple")' teosinte_joined.txt > teosinte_9_decrease.txt | sed -i "s%$old%$new2%g" teosinte_9_decrease.txt | sort -n -r -k 3 teosinte_9_decrease.txt -o teosinte_9_decrease.txt

awk '(NR>0) && ($2 == 10) && ($3 != "multiple")' teosinte_joined.txt > teosinte_10_decrease.txt | sed -i "s%$old%$new2%g" teosinte_10_decrease.txt | sort -n -r -k 3 teosinte_10_decrease.txt -o teosinte_10_decrease.txt

awk '(NR>0) && ($2 == "unknown")' teosinte_joined.txt > teosinte_unknown.txt

awk '(NR>0) && ($2 == "multiple") || ($3 == "multiple")' teosinte_joined.txt > teosinte_multiple.txt

# I will finish by saying that I am sure this code looks long and that incorporating a for loop for i in 1:10 for chromosome number and then condensing code would be a much cleaner option. I, for the life of me, can not get this to condense at the current time, but will continue in my attempts. #
