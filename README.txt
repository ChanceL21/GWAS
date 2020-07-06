General

This Python program finds associations between the functional state of genes and a given continuous or binary
phenotype in the 1135 Arabidopsis thaliana accessions of the 1001 Genomes Project. This is done by inputting
accession ID numbers and their corresponding phenotype values in the form of an excel file.

Set Up

Microsoft excel is required. This can be run on Ubuntu, but LibreOffice Calc will not work because it
cannot load excel files with a large number of columns, so Windows is recommended. In order to run this
program, Python 3 needs to be installed on your computer. Once installed, the following modules need to
be installed by typing 'pip install (module)' in command prompt:

•	numpy
•	pandas
•	xlrd
•	openpyxl
•	sklearn
•	statsmodels*

*statsmodels may require Microsoft Visual C++ 14.0 2017 to install properly on Windows.
The excel files ‘genotype_matrix.xlsx’ and ‘kinship_matrix.xlsx’ also need to be in the same
directory as the Python file ‘gwas.py’ and your input excel file.

Excel Input

Starting from the top left corner of the excel sheet, the ID’s and phenotypes should be entered
following this example format:

ID	Phenotype
88	11.2
159	12. 7
200	10.5
88	9.3
5	14.1

It is not necessary to have these exact header names, but headers above the data (in boxes A1 and B1)
are required. The accession ID numbers do not need to be in numerical order. If duplicate ID’s are entered
into the spreadsheet, then their phenotype data will be averaged. So in the above example, the ID 88 data
will be averaged as one data point of (11.2 + 9.3)/2 = 10.25. If an ID is entered that there is not
genotype data for, then it will be ignored. If the phenotype is binary instead of continuous, then the
two categories should be encoded as 0 and 1 in the table beforehand. Note: There can be nothing else in
the excel file except these two columns with the default sheet name of ‘Sheet1’ (case-sensitive). If your
data table is in a different file type than excel, then you can open a new excel workbook and import it by
going to ‘Data’ then ‘From Text’. Once the window opens, select ‘All Files’ in the drop down near the bottom
right, then select your data file and click ‘Import’. From here you can select the ‘Delimited’ option and
choose the divider (comma, space, etc.) of your file. Finally, select the A1 box as the import location.
Realistically, there needs to be phenotypes from > 400 accessions to get meaningful results.

Execution

To execute the program, navigate to the directory containing the four files mentioned above in command prompt
by entering ‘CD ex_directory1\ex_directory2’ and then entering ‘python gwas.py’. It will then prompt you to
type in the name of your excel file. For example, if an excel workbook is named ‘example’, then you should
enter ‘example.xlsx’. The program will then run for about a minute. There will sometimes be error text
printed in the terminal (usually a symptom of inputting a low number of accessions), but the program should
not crash. The input excel file must not be opened while the program is running. When finished, the number
of unique accessions with genotype data will be printed in the terminal.

Interpretation of Output

A new sheet in the input excel workbook will be created titled ‘Results’ that contains a table of each gene
ATG, B coefficient, and p value from the logistic regression model. Each model is composed of
gene = constant + phenotype + PC1 + PC2 + PC3, but only the B coefficient and p value of the phenotype
parameter are displayed. The gene is labelled as 0 (functional) or 1 (non-functional) for each accession in
the genotype matrix, so logistic regression is appropriate as the model essentially assesses the phenotype’s
ability to predict the functional state of each gene. PC 1, 2 and 3 are the first 3 Principal components of
a PCA on the kinship matrix of the accessions intended to correct for population structure. Some genes may
not have a B coefficient or p value if not enough accessions were analyzed. P values highlighted in orange
are significant (.05/2088), while p values highlighted in red are highly significant (.001/2088). The
significance threshold is modified through a Bonferroni correction to account for multiple comparisons,
since 2088 logistic models are evaluated (1 for each of 2088 genes). Positive B coefficients indicate that
knocking out a gene is associated with an increase in the phenotype unit, while negative B coefficients
indicate that knocking out a gene is associated with a decrease in the phenotype unit.

The method, logic, and much of the data used in this program came from 10.7554/eLife.41038 by Monroe et al. 
Feel free to email me at ChanceL21@comcast.net if you have any questions or find any bugs.
