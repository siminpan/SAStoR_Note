# SAStoR_Note
Notes for SAS to R

#### Read csv file ####

```SAS
data work.NewSalesEmps;
    length First_Name $ 12 Last_Name $ 18 Job_Title $ 25;
    infile 'newemps.csv' dlm=',';
    input First_Name $ Last_Name $ Job_Title $ Salary;
run;
```

```r
work.NewSalesEmps <- read.csv('newemps.csv',
                              col.names = c("First_Name", "Last_Name" $ "Job_Title", "Salary")
                            )
```

#### Print the data ####

```SAS
proc print data=work.NewSalesEmps;
run;
```

Use head() for checking the first few rows
Use View() to invoke a spreadsheet-style data viewer
```r
print(work.NewSalesEmps)
```

#### Print Some means ####

```SAS
proc means data=work.NewSalesEmps;
    class Job_Title;
    var Salary;
run;
```
Base R
No number of Observations
```r
aggregate(work.NewSalesEmps$Salary, list(work.NewSalesEmps$Job_Title),
          FUN=function(x) c(mn = mean(x), sd = sd(x), min = min(x), max = max(x) )
        )
```
Using dplyr
```r
library(dplyr)
work.NewSalesEmps %>%
  group_by(Job_Title) %>%
  summarise_at(vars(Salary),
              list(n = length, mean = mean, sd = sd, min = min, max = max)
  )
```
