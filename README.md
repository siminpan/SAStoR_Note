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

R can save image, pdf, html, or markdown file.
```r

png(' filename.ext ' <options>)
 code to generate a report(s)
dev.off()

# or ggsave() if it is a ggplot (or 
# other grid object).
```

</td>
</tr>
</table>

###  ###
SAS Code:
```SAS

run;
```
R Code:
```r

```

###  ###
SAS Code:
```SAS

run;
```
R Code:
```r

```

###  ###
SAS Code:
```SAS

run;
```
R Code:
```r

```

###  ###
SAS Code:
```SAS

run;
```
R Code:
```r

```

SAS Code:
```SAS
    
proc print data=work.NewSalesEmps noobs;
    var Last_Name First_Name Salary;
run;
```
R Code:
```r
    
work.NewSalesEmps[,c("Last_Name","First_Name","Salary")]
```
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
