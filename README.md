# Quiz Question 1 Selecting lines from stdin

There are 2 files with this task main file from which the lines have to be pulled and the identifier file where the content which must be there in the main file is present<br>
For better understanding , ``` head data/to_select.tsv``` will give you the file which contains the elemnts which should be present in the main file:<br>
```
ENSG00000180353.10
ENSG00000180596.7
ENSG00000266379.6
ENSG00000262561.1
ENSG00000284999.1
ENSG00000274641.1
ENSG00000231441.1
ENSG00000267655.1
ENSG00000209082.1
```
To see the main file ```zcat data/q1_data.tsv.gz|head```<br>
```
transcript_id(s)	gene_id	length	effective_length	expected_count	TPM	FPKM	posterior_mean_count	posterior_standard_deviation_of_count	pme_TPM	pme_FPKM	TPM_ci_lower_bound	TPM_ci_upper_bound	TPM_coefficient_of_quartile_variation	FPKM_ci_lower_bound	FPKM_ci_upper_bound	FPKM_coefficient_of_quartile_variation
10904	10904	93.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0	0	0	0	0	0
12954	12954	94.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0	0	0	0	0	0
12956	12956	72.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0	0	0	0	0	0
12958	12958	82.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0	0	0	0	0	0
12960	12960	73.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0	0	0	0	0	0
12962	12962	72.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0	0	0	0	0	0
12964	12964	74.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0.00	0	0	0	0	0	0
```
Now the question mentioned that the query in to_select.tsv can we anywhere and is not restricted to 2nd column so i read the main file line wise<br>
Firstly the contents of to_select.tsv were saved to a list, and while reading the main file , the comtent of 1st line was saved for column header of output file<br>
Then the the query was matched and only those lines were written to output files which had matching content and so whole file was scanned<br>
And finally to the output i added the column headers so that it looks well structured<br>

To implement this code write this on the terminal:

```
zcat data/q1_data.tsv.gz|python3 q1_code.py data/to_select.tsv q1_output.tsv
```

# Quiz Question 2 Plotting a group of lines

The standard input file is q2_data.tsv which when using linux command ```cat data/q2_data.tsv|head``` will give you an outlook of the data:<br>
```
-1118	1.27553239271999	Cl1
-1117	1.27696343296042	Cl1
-1116	1.27829211888462	Cl1
-1115	1.27953030556019	Cl1
-1114	1.28067189848055	Cl1
-1113	1.28170721322425	Cl1
-1112	1.28264434797129	Cl1
-1111	1.28348705960928	Cl1
-1110	1.28422232426115	Cl1
-1109	1.28485247954583	Cl1
```
The main aim is to plot the x and y values for different categories in the 3rd column using ggplot. The working of the code is simple we first read the standard input file and assign column names to them<br>
While using ggplot in the aes argument with x and y axis color was set to the third column , and rest of the labels and output file were given are arguments to the code

To obtain the desired plot:

```
cat data/q2_data.tsv |Rscript q2_code.r "different_clusters.png" "Relative from center [bp]" "Enrichment over Mean" "MNase fragment profile"
```

# Quiz Question 3 Merge multiple files
This objective of this code is to generalize the merging of multiple files on the first column values, all you have to do it to change the file names in the data/list_q3.tsv file<br>

In this example there are 2 files to be merged ,path to these files are stored in data/list_q3.tsv<br>

To see the contents of data/list_q3.tsv use ``` cat data/list_q3.tsv``` and you will see this output:<br>

data/q3_first.tsv<br>
data/q3_second.tsv<br>

To have a small glimpse at the individual files use ```head data/q3_first.tsv``` and ```head data/q3_second.tsv```.<br>

To run the code for this particular example we have used output file as join_output.tsv but you can write the desired output name.<br>

``` 
Rscript join_list_of_files.R data/list_q3.tsv > join_output.tsv
```

A little detail about the code , for initializing we have taken the 1st file and stored in the merged_data . Since the files have no column names , col_names have been set to false and default 'V1','V2" and so on will be assigned<br>

Now rest of the file list except the first one is iterated using for loop and the current_data is merged with the merged_data using inner_join(function) which takes the two files and by as arguments and later the merged_data was written to a stdout which was stored in join_output.tsv<br>


# Quiz Question 4 Quantile distribution

The standard input file is q4_data.tsv which when using linux command ```head data/q4_data.tsv``` will give you this output:<br>
```
43
25
27
40
2
26
12
95
66
39
```
So the workflow is to cat the input file shown above to our group_in_quantiles.py file which will take the number of quantiles input from the user in the form of argument vector<br>


The code will save the result to a output file "distributed_quantiles.tsv" , in this repo the file "distributed_quantiles.tsv" is the distribution of the input file data into 5 quantiles<br>

code for this file is 

```
cat data/q4_data.tsv |python3 group_in_quantiles.py 5
```
The python code is structured in such a way that it can take numbers according to user's desire and label the data with quantiles <br>

The logic behind the code is simple , the lines from standard input are read one by one and is stored in a list first and then a pandas dataframe is made out of this list.<br>

Using the ```qcut``` function of pandas we get 2 arrays as output one is the quantiles and other is the edge values of those quantiles.<br>

The quantiles array was made a new column of the same data frame. In the 2nd step the labels were also generalized according to user input using a list comprehension code.<br>

Finally i constructed a dictionary with the quartiles and their respective range, using a for loop this was also generalized because in the edge values list their will always be n+1 values , n being the input by user<br>

Accessing the quantiles column one by one their respective range was stored in a new list since list are ordered. Finally a range column was added to the dataframe with the values from this new list<br>

While saving index and header were set to None recreate the required output

# Quiz Question 5 Plotting big matrix

The objective of this problem was to understand the advantage of gnuplot over ggplot and matplotlib when it comes to huge chunks of data<br>

To download the big data file :<br>
```
curl -JLO https://figshare.com/ndownloader/files/49000657?private_link=9f1324117c2f6e734f2b -O big_data.tsv.gz
```
To make it better suited for gnuplot code the first column has to be removed first<br>
```
zcat big_data.tsv.gz |cut -f2- > plotting_data.tsv
```
These files have not been uploaded here due to their big size<br>
For the ease of debugging the code , i have taken just a small portion of the big file and tried to plot it<br>

Step1:<br>
```
cat plotting_data.tsv| head -n 1000 > demo_data.tsv
```
Step2:<br>
```
gnuplot -e "input_file = 'demo_data.tsv';output_file = 'demo.eps'" plotter.gp
```
Step3:<br>
```
ps2pdf demo.eps demo.pdf
```
Make sure gnuplot is installed in your sysytem <br>

For this particular example i have only uploaded the demo.pdf because in the main file while performing ```ps2pdf``` step it displayed an error <br>
```Fontconfig warning: ignoring UTF-8: not a valid region tag```<br>
After checking i corrected the configuration many times but still the same error persist , due to this error my eps file was not converted to pdf properly and i always recieved and empty image<br>
I fixed the input file by structuring it properly as a mtrix but still the same issue<br>



