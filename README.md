# SAStoR_Note
Notes for SAS to R

SAS statements have these characteristics:
* usually begin with an identifying keyword
* always end with a semicolon

Add comments:
SAS: /* comment */ or * comment ;

R: # comment

SAS Date Values:
A SAS date value is stored as the number of days between January 1, 1960, and a specific date. That date is 0 in SAS.

### Read csv file ###

<table>
<tr>
<th>SAS Code</th>
<th>R Code</th>
</tr>
<tr>
<td>
<pre>

```SAS

data work.NewSalesEmps;
    length First_Name $ 12 Last_Name $ 18 Job_Title $ 25;
    infile 'newemps.csv' dlm=',';
    input First_Name $ Last_Name $ Job_Title $ Salary;
run;
```

</pre>
</td>
<td>

```r
    
work.NewSalesEmps <- read.csv('newemps.csv',
                              col.names = c("First_Name", "Last_Name", 
                                            "Job_Title", "Salary")
                            )
```

</td>
</tr>
</table>

### Print the data ###

<table>
<tr>
<th>SAS Code</th>
<th>R Code</th>
</tr>
<tr>
<td>
<pre>

```SAS

proc print data=work.NewSalesEmps;
run;
```

</pre>
</td>
<td>

```r

print(work.NewSalesEmps)
# Use head() for checking the first few rows
# Use View() to invoke a spreadsheet-style data viewer
```

</td>
</tr>
</table>


### Print Some Summarise ###

<table>
<tr>
<th>SAS Code</th>
<th>R Code</th>
</tr>
<tr>
<td>
<pre>

```SAS

proc means data=work.NewSalesEmps;
    class Job_Title;
    var Salary;
run;
```

</pre>
</td>
<td>

```r

# Base R, No number of Observations
aggregate(work.NewSalesEmps$Salary, list(work.NewSalesEmps$Job_Title),
          FUN=function(x) c(mn = mean(x), sd = sd(x), 
                            min = min(x), max = max(x) )
        )
    
# Using dplyr
library(dplyr)
work.NewSalesEmps %>%
  group_by(Job_Title) %>%
  summarise_at(vars(Salary),
              list(n = length, mean = mean, sd = sd, 
                   min = min, max = max)
              )
```

</td>
</tr>
</table>


### Browsing the Descriptor Portion ###

<table>
<tr>
<th>SAS Code</th>
<th>R Code</th>
</tr>
<tr>
<td>
<pre>

```SAS

proc contents data=work.NewSalesEmps;
run;
```

</pre>
</td>
<td>

```r

# There is no easy way for R
str(work.NewSalesEmps)
names(work.NewSalesEmps)
dim(work.NewSalesEmps)
class(work.NewSalesEmps)
```

</td>
</tr>
</table>


### Browsing the Data Portion ###

<table>
<tr>
<th>SAS Code</th>
<th>R Code</th>
</tr>
<tr>
<td>
<pre>

```SAS

proc print data=work.NewSalesEmps noobs;
    var Last_Name First_Name Salary;
run;
```

</pre>
</td>
<td>

```r

work.NewSalesEmps[,c("Last_Name","First_Name","Salary")]
```

</td>
</tr>
</table>

### Browsing a SAS Data Library ###
LIBNAME libref 'SAS-data-library' <options>;
LIBNAME libref engine-name <SAS/ACCESS-options>;

FILENAME fileref ‘external-file' <options>;

* Libref (libname) = alias to a collection of tables
* Fileref (filename) = alias to a single file
* Raw text files are not considered tables and cannot be

SAS Code:
```SAS
PROC CONTENTS DATA=libref._ALL_ NODS;
run;
```
No R equivalent

SAS Code:
```SAS
libname oralib oracle
    user=edu001 pw=edu001
    path=dbmssrv schema=educ;
run;
libname oralib clear;
```
The LIBNAME statement remains in effect until canceled, changed, or your SAS session ends.

### ODS Destination ###
The LISTING destination was the default ODS destination until late 9.2.
Since then it has been HTML.

| Destination | Type of File | Viewed In |
| ----------- | ------------ | --------- |
| LISTING | | SAS Output Window or SAS/GRAPH Window |
| HTML | Hypertext Markup Language | Web Browsers such as Internet Explorer |
| PDF | Portable Document Format | Adobe Products such as Acrobat Reader |
| RTF | Rich Text Format | Word Processors such as Microsoft Word |
|CSVALL|Comma-Separated Value|Editor or Microsoft Excel|
|MSOFFICE2K|Hypertext Markup Language|Web Browser or Microsoft Word or Microsoft Excel|
|EXCELXP|Extensible Markup Language|Microsoft Excel|
|EXCEL|True Excel File|Microsoft Excel|
    
ODS EXCEL produces a native XLSX file.
ODS EXCEL supports SAS graphics in the output.
The ODS LISTING CLOSE statement stops sending output to the OUTPUT and GRAPH windows.


<table>
<tr>
<th>SAS Code</th>
<th>R Code</th>
</tr>
<tr>
<td>
<pre>

```SAS

ODS destination FILE = ' filename.ext ' <options>;
  SAS code to generate a report(s)
ODS destination CLOSE;
```
It is a good habit to open the LISTING 
destination at the end of a program to 
guarantee an open destination for the 
next submission.
Output can be sent to many destinations. 
Use _ALL_ in the ODS CLOSE statement 
to close all open destinations including 
the LISTING destination.

</pre>
</td>
<td>

R can save csv, image, pdf, html, or markdown file.
```r

png(' filename.ext ' <options>)
 code to generate a report(s)
dev.off()

# or ggsave() if it is a ggplot (or 
# other grid object).

write.table(x, file = "", append = FALSE, quote = TRUE, sep = " ",
            eol = "\n", na = "NA", dec = ".", row.names = TRUE,
            col.names = TRUE, qmethod = c("escape", "double"),
            fileEncoding = "")
write.csv(...)
```

</td>
</tr>
</table>

### Style Definitions ###
SAS Code:
```SAS

ODS destination FILE = 'filename.ext'
    STYLE = style-definition;
```
    
SAS Supplied Style Definitions
|   |   |   |   |
|---|---|---|---|
|Analysis|Astronomy|Banker|BarrettsBlue|
|Beige|blockPrint|Brick|Brown|
|Curve|D3d|Default|Education|
|EGDefault|Electronics|fancyPrinter|Festival|
|FestivalPrinter|Gears|Journal|Magnify|
|Meadow|MeadowPrinter|Minimal|Money|
|NoFontDefault|Normal|NormalPrinter|Printer|
|Rsvp|Rtf|sansPrinter|sasdocPrinter|
|Sasweb|Science|Seaside|SeasidePrinter|
|serifPrinter|Sketch|Statdoc|Statistical|
|Theme|Torn|Watercolor|grayscalePrinter|
|Harvest|HighContrast|Journal2|Journal3|
|Listing|monochromePrinter|Ocean|Solutions|

### Reading Data ###
    
<table>
<tr>
<th> </th>
<th>Reading SAS Data Sets</th>
<th>Reading Excel Worksheets</th>
<th>Reading Delimited Raw Data Files</th>
</tr>
<tr>
<td>
<pre>

SAS Code

</pre>
</td>
<td>

```SAS

LIBNAME libref 'SAS-data-library';

DATA output-SAS-data-set;
    SET input-SAS-data-set;
    WHERE where-expression;
    KEEP variable-list;
    LABEL variable = 'label'
          variable = 'label'
          variable = 'label';
    FORMAT variable(s) format;
RUN;
```

</td>
<td>

```SAS

libname ;
data ;
    set ;
    ...
run;
```

</td>
<td>

```SAS

data ;
    infile ;
    input ;
    ...
run;
```

</td>
</tr>
<tr>
<td>
<pre>

R Code

</pre>
</td>
<td>

```r

library("haven")
df <- read_sas("output-SAS-data.sas7bdat")

library("sas7bdat")
df <- read.sas("output-SAS-data.sas7bdat")
```

</td>
<td>

```r

library("readxl")
# xls files
my_data <- read_excel("my_file.xls", sheet = "data")
# xlsx files
my_data <- read_excel("my_file.xlsx", sheet = 2)

library("xlsx")
read.xlsx(file, sheetIndex, header=TRUE)
read.xlsx2(file, sheetIndex, header=TRUE)
```

</td>
<td>

```r

read.csv()
read_csv()
```

</td>
</tr>
</table>
    
### The WHERE Statement ###

<table>
<tr>
<th>SAS Code</th>
<th>R Code</th>
</tr>
<tr>
<td>
<pre>

```SAS

WHERE where-expression ;
where Gender = 'M';
where Salary > 50000;

```

</pre>
</td>
<td>

```r

df[which(df$Salary > 50000),]
```

</td>
</tr>
</table>

### Comparison Operators ###
|Definition|SAS Symbol|SAS Mnemonic|R Symbol|
|---|:-:|:-:|:-:|
|equal to|=|EQ|==|
|not equal to|^= ¬= ~=|NE|!=|
|greater than|>|GT|>|
|less than|<|LT|<|
|greater than or equal|>=|GE|>=|
|less than or equal|<=|LE|<=|
|equal to one of a list||IN|%in%|

### Arithmetic Operators ###
|Definition|SAS Symbol|R Symbol|
|---|:-:|:-:|
|exponentiation|**|^ or **|
|multiplication|*|*|
|division|/|/|
|addition|+|+|
|subtraction|-|-|
|modulus|mod()|%%|
|integer division||%/%|

### Logical Operators ###
|Definition|SAS Symbol|SAS Mnemonic|R Symbol|
|---|:-:|:-:|:-:|
|logical and|&|AND|&|
|logical or|\||OR|\||
|logical not|^ ¬ ~|NOT|!|
    
 ### Special WHERE Operators ###
 
Special WHERE operators are operators that can only be used in a
where-expression.

|Definition|SAS Symbol|SAS Mnemonic|R Symbol|
|---|:-:|:-:|:-:|
|an inclusive range||BETWEEN-AND|10<=x & x<=15|
|null value||IS NULL|is.null(x)|
|missing value||IS MISSING|is.missing(x)|
|a character string|?|CONTAINS|grepl("\\bWord1\\b",c("Word1","Word2","Word12"))|
||||grepl("\\<Word1\\>",c("Word1","Word2","Word12"))|
|a character pattern||LIKE|grep functions|

### The DROP and KEEP Statements ###

<table>
<tr>
<th>SAS Code</th>
<th>R Code</th>
</tr>
<tr>
<td>
<pre>

```SAS

DROP variable-list;
KEEP variable-list;
```

</pre>
</td>
<td>

```r

# for loaded data
df[,c(variable-list)]
# for reading new data
x<- read.csv(file="datat.csv",
               colClasses = c(rep("character", 2),
                              rep("numeric", 1),
                              rep("NULL", 3)),
               header = T)

```

</td>
</tr>
</table>
    
### LABEL Statement ###

* a label changes the appearance of a variable name
* a format changes the appearance of variable value.

<table>
<tr>
<th>SAS Code</th>
<th>R Code</th>
</tr>
<tr>
<td>
<pre>

```SAS

LABEL variable = 'label'
      variable = 'label'
      variable = 'label';
```

</pre>
</td>
<td>

```r

# In R there is only rename
names(df1) <- colnames(df2)
colnames(df1) <- colnamelist

library(dplyr)
df1 %>% rename_all(~df2 %>% pull(col))
# where col is the column name in df2 dataframe.
```

</td>
</tr>
</table>

In order to use labels in the PRINT procedure, a LABEL option needs to be
added to the PROC PRINT statement.

```SAS

proc print data=work.subset1 label;
run;
```

### FORMAT Statement ###

<table>
<tr>
<th>SAS Code</th>
<th>R Code</th>
</tr>
<tr>
<td>
<pre>

```SAS

FORMAT variable(s) format;
```

</pre>
</td>
<td>

```r

formatC()
prettyNum()
format()
```

</td>
</tr>
</table>
    
SAS formats have the following form:
```SAS

<$>format<w>.<d>
```
|||
|:-:|---|
|$|indicates a character format.|
|format|names the SAS format or user-defined format.|
|w|specifies the total format width including decimal|
||places and special characters.|
|.|is a required delimiter.|
|d|specifies the number of decimal places in numeric formats.|

Selected SAS formats:

|Format|Definition|
|:-:|---|
|$w.|writes standard character data.|
|w.d|writes standard numeric data.|
|COMMAw.d|writes numeric values with a comma that separates every three digits and a period that separates the decimal fraction.|
|COMMAXw.d|writes numeric values with a period that separates every three digits and a comma that separates the decimal fraction.|
|DOLLARw.d|writes numeric values with a leading dollar sign, a comma that separates every three digits, and a period that separates the decimal fraction.|
|EUROXw.d|writes numeric values with a leading euro symbol (€), a period that separates every three digits, and a comma that separates the decimal fraction.|

|Format|Stored Value|Displayed Value|
|:-:|---|---|
|$4.|Programming|Prog|
|12.|27134.2864|27134|
|12.2|27134.2864|27134.29|
|COMMA12.2|27134.2864|27,134.29|
|COMMAX12.2|27134.2864|27.134,29|
|DOLLAR12.2|27134.2864|$27,134.29|
|EUROX12.2|27134.2864|€27.134,29|

If you do not specify a format width that is large enough to accommodate a
numeric value, the displayed value is automatically adjusted to fit into
the width.

|Format|Stored Value|Displayed Value|
|:-:|---|---|
|DOLLAR12.2|27134.2864|$27,134.29|
|DOLLAR9.2|27134.2864|$27134.29|
|DOLLAR8.2|27134.2864|27134.29|
|DOLLAR5.2|27134.2864|27134|
|DOLLAR4.2|27134.2864|27E3|

SAS Date Formats
Format|Stored Value|Displayed Value|
|:-:|--:|--:|
|MMDDYY6.|0|010160|
|MMDDYY8.|0|01/01/60|
|MMDDYY10.|0|01/01/1960|
|DDMMYY6.|365|311260|
|DDMMYY8.|365|31/12/60|
|DDMMYY10.|365|31/12/1960|
|DATE7.|-1|31DEC59|
|DATE9.|-1|31DEC1959|
|WORDDATE.|0|January 1, 1960|
|WEEKDATE.|0|Friday, January 1, 1960|
|MONYY7.|0|JAN1960|
|YEAR4.|0|1960|
