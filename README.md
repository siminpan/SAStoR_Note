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

SAS Code:
```SAS
data work.NewSalesEmps;
    length First_Name $ 12 Last_Name $ 18 Job_Title $ 25;
    infile 'newemps.csv' dlm=',';
    input First_Name $ Last_Name $ Job_Title $ Salary;
run;
```

R Code:
```r
work.NewSalesEmps <- read.csv('newemps.csv',
                              col.names = c("First_Name", "Last_Name" $ "Job_Title", "Salary")
                            )
```

### Print the data ###

SAS Code:
```SAS
proc print data=work.NewSalesEmps;
run;
```

Use head() for checking the first few rows
Use View() to invoke a spreadsheet-style data viewer
R Code:
```r
print(work.NewSalesEmps)
```

### Print Some means ###

SAS Code:
```SAS
proc means data=work.NewSalesEmps;
    class Job_Title;
    var Salary;
run;
```
Base R
No number of Observations
R Code:
```r
aggregate(work.NewSalesEmps$Salary, list(work.NewSalesEmps$Job_Title),
          FUN=function(x) c(mn = mean(x), sd = sd(x), min = min(x), max = max(x) )
        )
```
Using dplyr
R Code:
```r
library(dplyr)
work.NewSalesEmps %>%
  group_by(Job_Title) %>%
  summarise_at(vars(Salary),
              list(n = length, mean = mean, sd = sd, min = min, max = max)
  )
```

### Browsing the Descriptor Portion ###
SAS Code:
```SAS
proc contents data=work.NewSalesEmps;
run;
```
No easy way for R
R Code:
```r
str(df)
names(df)
dim(df)
class(df)
```
### Browsing the Data Portion ###
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

### Default ODS Destination ###
The LISTING destination was the default ODS destination until late 9.2.
Since then it has been HTML.

| Destination | Type of File | Viewed In |
| ----------- | ------------ | --------- |
| LISTING | | SAS Output Window or SAS/GRAPH Window |
| HTML | Hypertext Markup Language | Web Browsers such as Internet Explorer |
| PDF | Portable Document Format | Adobe Products such as Acrobat Reader |
| RTF | Rich Text Format | Word Processors such as Microsoft Word |

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

###  ###
SAS Code:
```SAS

run;
```
R Code:
```r

```

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
