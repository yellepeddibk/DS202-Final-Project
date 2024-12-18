
# Analysis on COVID Impact on Work

#### Bhargav Yellepeddi, Neel Rajan, Amaya Bayoumi, Ananya Ramji

## Introduction

In this analysis, we explore a dataset detailing information about how
covid had an impact on people’s work lives.

The goal of this project is to explore how Pandemics like COVID can
impact various aspects of our lives. When the COVID-19 Pandemic
occurred, society as a whole was not at all prepared on how to react,
which caused lots of famine in many counties and sent the world into
chaos. By analyzing this dataset, people can better understand what
areas of life will be impacted the most, and thus learn how to better
prepare for future events such as COVID. Ultimately, our goal is to
determine the best methods that can minimize the impacts of
Pandemic-like events.

To achieve this objective, we will explore the following questions:

1.  What cleaning methods were applied, and why?

2.  What is the distribution of the “Productivity_Change” and
    “Stress_Level” Variables?

3.  What is the impact of working from home on productivity change?

- Stress Level & Working from Home
- Sector
- Sector & Working from Home
- Childcare Responsibilities & Working from Home
- Health Issues & Working from Home

4.  How does Stress_Level impact Productivity_Change when and when not
    working from Home?

5.  Do employees who experienced salary cuts show different productivity
    trends compared to those with stable salaries?

6.  What was the impact of Childcare Responsibilities, Commuting
    Changes, and Health Issues on Technology Adaptation Levels

7.  What are the key takeaways or recommendations based on our analysis?

8.  What can we improve if this analysis were conducted again?

These are the primary questions we seek to answer in this final project.
From the information drawn from these questions, we will be able to
derive meaningful insights on the best practices to use during Pandmeics
and better understand how to prepare for them.

## Data

### Structure

    The link to the data set is the following: https://www.kaggle.com/datasets/willianoliveiragibin/covid-19-on-working-professionals/data. The Kaggle website constains a CSV file containing all the data collected to detail the impact COVID had on people's work lives. There are 15 columns for 15 variables, where each variable describes a different aspect of work and whether or not it has had any change or effect from COVID. All of the data-points are neatly organized into one CSV file, where there are exactly 10,000 rows.

    One of the most important things to note about this dataset is that fact that most of the variables report their data in a binary format. In other words, most of the data is listed in 0's and 1's, where 0 conveys that the given data-point has not been effected or changed for its given variable/column, while 1 conveys that it has. Although this may seem subtle, this has lead to a variety of issues and mis-interpretations throughout this project, which have been since fixed of course. 

    Where normally graphs depict the amount of change a variable has had, binary-based variables will depict whether or not there was any change and how much of the data has changed. When it comes to this project specifically, this is actually a valid approach as all we need to see is what variables changed in response to COVID-19. The variables that had less change indicate that they are more resilient and are of less worry, whereas the variables that have a higher proportion of change are the ones that indicate the need for more caution and preparation.

    With that said, we now move on to viewing the dataset and gaining an overall synopsis of what to expect:

``` r
# View the data 
head(data)
```

    ## # A tibble: 6 × 15
    ##   Stress_Level Sector   Increased_Work_Hours Work_From_Home Hours_Worked_Per_Day
    ##   <chr>        <chr>                   <dbl>          <dbl> <chr>               
    ## 1 Low          Retail                      1              1 6.392.393.639.805.8…
    ## 2 Low          IT                          1              1 9.171.983.537.957.5…
    ## 3 Medium       Retail                      1              0 10.612.560.951.456.…
    ## 4 Medium       Educati…                    1              1 5.546.168.647.409.5…
    ## 5 Medium       Educati…                    0              1 11.424.615.456.733.…
    ## 6 Low          IT                          1              1 7.742.897.931.229.7…
    ## # ℹ 10 more variables: Meetings_Per_Day <chr>, Productivity_Change <dbl>,
    ## #   Health_Issue <dbl>, Job_Security <dbl>, Childcare_Responsibilities <dbl>,
    ## #   Commuting_Changes <dbl>, Technology_Adaptation <dbl>, Salary_Changes <dbl>,
    ## #   Team_Collaboration_Challenges <dbl>, Affected_by_Covid <dbl>

### Cleaning

#### Overview the Data

First, we will take an overview of the Data to see what aspects needed
cleaning.

``` r
colnames(data) # View the column names to ensure proper references
```

    ##  [1] "Stress_Level"                  "Sector"                       
    ##  [3] "Increased_Work_Hours"          "Work_From_Home"               
    ##  [5] "Hours_Worked_Per_Day"          "Meetings_Per_Day"             
    ##  [7] "Productivity_Change"           "Health_Issue"                 
    ##  [9] "Job_Security"                  "Childcare_Responsibilities"   
    ## [11] "Commuting_Changes"             "Technology_Adaptation"        
    ## [13] "Salary_Changes"                "Team_Collaboration_Challenges"
    ## [15] "Affected_by_Covid"

As shown, there are 15 variables/columns.

``` r
str(data)      # Structure of the data
```

    ## spc_tbl_ [10,000 × 15] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ Stress_Level                 : chr [1:10000] "Low" "Low" "Medium" "Medium" ...
    ##  $ Sector                       : chr [1:10000] "Retail" "IT" "Retail" "Education" ...
    ##  $ Increased_Work_Hours         : num [1:10000] 1 1 1 1 0 1 0 1 1 1 ...
    ##  $ Work_From_Home               : num [1:10000] 1 1 0 1 1 1 0 1 1 1 ...
    ##  $ Hours_Worked_Per_Day         : chr [1:10000] "6.392.393.639.805.820" "9.171.983.537.957.560" "10.612.560.951.456.400" "5.546.168.647.409.510" ...
    ##  $ Meetings_Per_Day             : chr [1:10000] "26.845.944.014.488.700" "33.392.245.834.602.800" "2.218.332.712.302.110" "5.150.566.193.312.910" ...
    ##  $ Productivity_Change          : num [1:10000] 1 1 0 0 1 1 0 0 0 0 ...
    ##  $ Health_Issue                 : num [1:10000] 0 0 0 0 0 1 0 1 1 1 ...
    ##  $ Job_Security                 : num [1:10000] 0 1 0 0 1 0 0 0 1 0 ...
    ##  $ Childcare_Responsibilities   : num [1:10000] 1 0 0 0 1 1 1 0 0 1 ...
    ##  $ Commuting_Changes            : num [1:10000] 1 1 0 1 1 1 0 1 0 1 ...
    ##  $ Technology_Adaptation        : num [1:10000] 1 1 0 0 0 1 1 1 0 1 ...
    ##  $ Salary_Changes               : num [1:10000] 0 0 0 0 1 0 0 0 0 0 ...
    ##  $ Team_Collaboration_Challenges: num [1:10000] 1 1 0 0 1 1 1 1 1 1 ...
    ##  $ Affected_by_Covid            : num [1:10000] 1 1 1 1 1 1 1 1 1 1 ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   Stress_Level = col_character(),
    ##   ..   Sector = col_character(),
    ##   ..   Increased_Work_Hours = col_double(),
    ##   ..   Work_From_Home = col_double(),
    ##   ..   Hours_Worked_Per_Day = col_character(),
    ##   ..   Meetings_Per_Day = col_character(),
    ##   ..   Productivity_Change = col_double(),
    ##   ..   Health_Issue = col_double(),
    ##   ..   Job_Security = col_double(),
    ##   ..   Childcare_Responsibilities = col_double(),
    ##   ..   Commuting_Changes = col_double(),
    ##   ..   Technology_Adaptation = col_double(),
    ##   ..   Salary_Changes = col_double(),
    ##   ..   Team_Collaboration_Challenges = col_double(),
    ##   ..   Affected_by_Covid = col_double()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

This code snippet reveals the structure of the Dataset. Some key things
to note are that the binary variables are all listed as numeric data
types while the rest are character data types.

``` r
summary(data)  # Summary statistics
```

    ##  Stress_Level          Sector          Increased_Work_Hours Work_From_Home  
    ##  Length:10000       Length:10000       Min.   :0.0000       Min.   :0.0000  
    ##  Class :character   Class :character   1st Qu.:0.0000       1st Qu.:1.0000  
    ##  Mode  :character   Mode  :character   Median :1.0000       Median :1.0000  
    ##                                        Mean   :0.6769       Mean   :0.8033  
    ##                                        3rd Qu.:1.0000       3rd Qu.:1.0000  
    ##                                        Max.   :1.0000       Max.   :1.0000  
    ##  Hours_Worked_Per_Day Meetings_Per_Day   Productivity_Change  Health_Issue   
    ##  Length:10000         Length:10000       Min.   :0.0000      Min.   :0.0000  
    ##  Class :character     Class :character   1st Qu.:0.0000      1st Qu.:0.0000  
    ##  Mode  :character     Mode  :character   Median :1.0000      Median :0.0000  
    ##                                          Mean   :0.5022      Mean   :0.3011  
    ##                                          3rd Qu.:1.0000      3rd Qu.:1.0000  
    ##                                          Max.   :1.0000      Max.   :1.0000  
    ##   Job_Security    Childcare_Responsibilities Commuting_Changes
    ##  Min.   :0.0000   Min.   :0.0000             Min.   :0.0000   
    ##  1st Qu.:0.0000   1st Qu.:0.0000             1st Qu.:0.0000   
    ##  Median :0.0000   Median :0.0000             Median :1.0000   
    ##  Mean   :0.4049   Mean   :0.3967             Mean   :0.5022   
    ##  3rd Qu.:1.0000   3rd Qu.:1.0000             3rd Qu.:1.0000   
    ##  Max.   :1.0000   Max.   :1.0000             Max.   :1.0000   
    ##  Technology_Adaptation Salary_Changes   Team_Collaboration_Challenges
    ##  Min.   :0.0000        Min.   :0.0000   Min.   :0.0000               
    ##  1st Qu.:0.0000        1st Qu.:0.0000   1st Qu.:0.0000               
    ##  Median :1.0000        Median :0.0000   Median :1.0000               
    ##  Mean   :0.6051        Mean   :0.1948   Mean   :0.7006               
    ##  3rd Qu.:1.0000        3rd Qu.:0.0000   3rd Qu.:1.0000               
    ##  Max.   :1.0000        Max.   :1.0000   Max.   :1.0000               
    ##  Affected_by_Covid
    ##  Min.   :1        
    ##  1st Qu.:1        
    ##  Median :1        
    ##  Mean   :1        
    ##  3rd Qu.:1        
    ##  Max.   :1

The summary statistics tells us quite a few things. One of the key
pieces of information to take away is the proportion of data-points that
were effected by their respective variables. This mainly applies to the
binary/numeric variable types as the character types simply list that
they are characters. The binary types, on the other hand, show their
mean values, which represents the proportion of data that was effected
in their column. For example, the “Job_Security” column lists a 0.4049
value for the mean, which means that around 40% of the people in the
data set experienced and impact or change to their job securities during
the impact. This indicates that job securtity was more stable than not
for the average person during the pandemic. In contrast,
“Team_Collaboration_Challenges” lists a mean value of 0.7006, meaning
that 70% of people in the dataset experinced team collaboration
challenges, indicating that this aspect is vulnerable to Pandmeic-like
events. Overall, this information alone provides valuable insights and
can allow society to better prepare for future events.

``` r
colSums(is.na(data)) # Check missing values
```

    ##                  Stress_Level                        Sector 
    ##                             0                             0 
    ##          Increased_Work_Hours                Work_From_Home 
    ##                             0                             0 
    ##          Hours_Worked_Per_Day              Meetings_Per_Day 
    ##                             0                             0 
    ##           Productivity_Change                  Health_Issue 
    ##                             0                             0 
    ##                  Job_Security    Childcare_Responsibilities 
    ##                             0                             0 
    ##             Commuting_Changes         Technology_Adaptation 
    ##                             0                             0 
    ##                Salary_Changes Team_Collaboration_Challenges 
    ##                             0                             0 
    ##             Affected_by_Covid 
    ##                             0

As shown here, all the columns have a 0 for null values, which means
that there are no empty values in any column. This means that we will
not have to worry about accounting for null values during the data
cleaning process.

``` r
unique(data)
```

    ## # A tibble: 10,000 × 15
    ##    Stress_Level Sector  Increased_Work_Hours Work_From_Home Hours_Worked_Per_Day
    ##    <chr>        <chr>                  <dbl>          <dbl> <chr>               
    ##  1 Low          Retail                     1              1 6.392.393.639.805.8…
    ##  2 Low          IT                         1              1 9.171.983.537.957.5…
    ##  3 Medium       Retail                     1              0 10.612.560.951.456.…
    ##  4 Medium       Educat…                    1              1 5.546.168.647.409.5…
    ##  5 Medium       Educat…                    0              1 11.424.615.456.733.…
    ##  6 Low          IT                         1              1 7.742.897.931.229.7…
    ##  7 Medium       IT                         0              0 6.049.957.230.122.9…
    ##  8 High         Health…                    1              1 9.515.509.560.416.3…
    ##  9 Medium       Educat…                    1              1 7.107.091.043.489.9…
    ## 10 High         Educat…                    1              1 7.836.526.647.937.0…
    ## # ℹ 9,990 more rows
    ## # ℹ 10 more variables: Meetings_Per_Day <chr>, Productivity_Change <dbl>,
    ## #   Health_Issue <dbl>, Job_Security <dbl>, Childcare_Responsibilities <dbl>,
    ## #   Commuting_Changes <dbl>, Technology_Adaptation <dbl>, Salary_Changes <dbl>,
    ## #   Team_Collaboration_Challenges <dbl>, Affected_by_Covid <dbl>

After a check for unique values, it is evident that there is an issue
with the columns “Hours_Worked_Per_Day” and “Meetings_Per_Day” as their
values do not align with typical numbers. However, the other columns
seem to have their proper data types and range of values.

#### Question 1: What cleaning methods were applied, and why?

From the information listed above, we realized that not much Data
Cleaning is necessary for the dataset: the binaary values are all
already listed as doubles, where numerics fall under; the verbal columns
have the proper “character” data type; the null check shows 0 null
values for all columns, which means we do not need to perform any
substitution or data removal; and lastly, from the unique input types,
except the Hours_Worked_Per_Day and Meetings_Per_Day columns, all column
indicate that they only have the data types they should have, such as
binary variables only have binary types. That of course leaves us with
the need to fix the Hours_Worked_Per_Day and Meetings_Per_Day columns.
The way the data is inputed in these columns is very interesting; it is
most likely formatted in a different countries numeric system,
especially with the number of decimals - ex. “6.392.393.639.805.820”. At
first, we thought that maybe each input was representing multiple days
of the week per person, but the column explicitly states
“Hours_Worked_Per_Day” or “Meetings_Per_Day” implying each input
describes the value per day. This means that something like hours should
range from 0-24 hours, which made us to believe that the data was
inputted without any sort of rounding, resulting in the long floating
values we see. However, after even further analysis, we found a negative
value inputted into the “Meetings_Per_Day” column:
-5.829.769.194.792.650. At this point, we could not find any reasoning
or explanattion that clarifies how this value could exist; it is not
possible to have negative meetings in a day. With seeing the unsusally
decimal input and the anomoly of negative meetings, we finally decided
that it was best to completely remove both these columns from the
dataset as it was evident that they were corrupted.

``` r
# Remove the two problematic columns
data <- data[, !(colnames(data) %in% c("Hours_Worked_Per_Day", "Meetings_Per_Day"))]

# Confirm the columns have been removed
print(colnames(data))
```

    ##  [1] "Stress_Level"                  "Sector"                       
    ##  [3] "Increased_Work_Hours"          "Work_From_Home"               
    ##  [5] "Productivity_Change"           "Health_Issue"                 
    ##  [7] "Job_Security"                  "Childcare_Responsibilities"   
    ##  [9] "Commuting_Changes"             "Technology_Adaptation"        
    ## [11] "Salary_Changes"                "Team_Collaboration_Challenges"
    ## [13] "Affected_by_Covid"

As shown, we no longer have the problematic columns and are left with 13
variables for 13 columns.

    ## Unique values in Stress_Level :
    ## [1] "Low"    "Medium" "High"  
    ## ----------------------
    ## Unique values in Sector :
    ## [1] "Retail"     "IT"         "Education"  "Healthcare"
    ## ----------------------
    ## Unique values in Increased_Work_Hours :
    ## [1] 1 0
    ## ----------------------
    ## Unique values in Work_From_Home :
    ## [1] 1 0
    ## ----------------------
    ## Unique values in Productivity_Change :
    ## [1] 1 0
    ## ----------------------
    ## Unique values in Health_Issue :
    ## [1] 0 1
    ## ----------------------
    ## Unique values in Job_Security :
    ## [1] 0 1
    ## ----------------------
    ## Unique values in Childcare_Responsibilities :
    ## [1] 1 0
    ## ----------------------
    ## Unique values in Commuting_Changes :
    ## [1] 1 0
    ## ----------------------
    ## Unique values in Technology_Adaptation :
    ## [1] 1 0
    ## ----------------------
    ## Unique values in Salary_Changes :
    ## [1] 0 1
    ## ----------------------
    ## Unique values in Team_Collaboration_Challenges :
    ## [1] 1 0
    ## ----------------------
    ## Unique values in Affected_by_Covid :
    ## [1] 1
    ## ----------------------

As shown above, each column lists the unique values that we expect to
see, where binary variables only have 0’s and 1’s, as they should, while
the character variables have their proper resepctive labels. At this
point, the data has been cleaned and we are ready to proceed with
further analysis.

### Variables

- Stress_Level: Indicates the employee’s stress level, categorized into
  levels such as low, moderate, or high.

- Sector: Specifies the industry or sector (e.g., Retail, IT) in which
  the employee works.

- Increased_Work_Hours: A binary variable (1/0) indicating whether the
  employee experienced an increase in work hours.

- Work_From_Home: A binary variable (1/0) showing whether the employee
  is working from home.

- Productivity_Change: A binary variable (1/0) indicating whether the
  employee experienced a change in productivity.

- Health_Issue: A binary variable (1/0) denoting whether the employee
  reported any health issues.

- Job_Security: A binary variable (1/0) indicating whether the employee
  feels secure in their job.

- Childcare_Responsibilities: A binary variable (1/0) indicating if the
  employee has childcare responsibilities.

- Commuting_Changes: A binary variable (1/0) showing whether the
  employee’s commute has changed.

- Technology_Adaptation: A binary variable (1/0) indicating if the
  employee had to adapt to new technology for work.

- Salary_Changes: A binary variable (1/0) representing whether the
  employee’s salary changed.

- Team_Collaboration_Challenges: A binary variable (1/0) indicating if
  the employee faced challenges collaborating with their team.

- Affected_by_Covid: A binary variable (1/0) specifying whether the
  employee’s work or life has been impacted by COVID-19.

Although we use most of these variables througout this analysis, our
variables of focus are going to be the “Productivity_Change” and
“Stress_Level” as well as “Technology_Adaptation” since these variables
will tell us how society is doing as a whole as they reflect the state
of the people.

#### Question 2: What is the distribution of the “Productivity_Change” and “Stress_Level” Variables?

![](README_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

Initially, we used a standard distribution graph to depict our binary
data. However, we soon realized that this was an incorrect choice, as
binary values do not follow a continuous distribution. Instead, they
require a discrete representation such as a bar chart or a specialized
binary plot which accurately captured the nature of 0s and 1s. As
illustrated in the graph, 4,978 participants reported no change in their
productivity, whereas 5,022 reported experiencing a change during the
pandemic. Although these results are quite close, they suggest a nearly
even split between individuals who felt a shift in productivity and
those who did not. We will delve deeper into these we will further see
where the changes occur. Now, let’s see the Stress_Level:

![](README_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

The Stress_Level variable is different from the Productivity_Change
variable in that it is not binary, but rather categorical. It made the
most sense to proceeed with a bar graph as we could depict each state of
stress and the number of people for each. The distribution indicates
that most people reported being at a medium stress level during the
Pandemic, which is what we expected. We will further analyze the dataset
to find out the effects of such stress levels, what causes them, and
what methods work in events like pandemics to minimize stress.

## Results

#### Question 3: What is the impact of working from home on productivity change?

##### Stress Level & Working from Home:

![](README_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

The graph shows the distribution of stress levels among employees,
segmented by their work arrangement (working from home or on-site).
Employees with medium stress levels make up the largest group, with a
significant portion working from home. While a higher count of remote
workers is seen across all stress levels, the graph does not suggest
that working from home causes increased stress. Instead, it highlights
the prevalence of stress levels regardless of work arrangement.

##### Sector:

![](README_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

The graph displays the average productivity change by sector during
COVID-19. Education (0.508) and IT (0.505) report the highest changes,
likely reflecting their ability to adapt quickly to remote work but also
highlighting greater disruption. In contrast, Healthcare (0.500) and
Retail (0.496) show lower average changes, suggesting these sectors
maintained more stability or resilience during the pandemic, possibly
due to the essential nature of their operations.

##### Sector & Working from Home:

![](README_files/figure-gfm/unnamed-chunk-14-1.png)<!-- -->

![](README_files/figure-gfm/unnamed-chunk-14-2.png)<!-- -->

The analysis shows that the number of reported productivity changes
varies by sector and work arrangement. Across all sectors, remote
workers report significantly more changes than on-site workers,
suggesting greater shifts in remote environments. For example, in IT,
1,023 remote employees reported changes compared to 238 on-site, while
in Education, the split is 997 remote versus 225 on-site.

Sector differences are also notable. IT and Education report the highest
number of changes, reflecting greater disruption. In contrast, Retail
and Healthcare show fewer reported changes overall, with 982 remote and
265 on-site for Retail, and 996 remote and 252 on-site for Healthcare.
This suggests a higher degree of resilience or stability in these
sectors during the pandemic.

Overall, fewer reported changes, particularly among on-site workers and
resilient sectors like Retail and Healthcare, indicate greater stability
during pandemic-like disruptions.

##### Childcare Responsibilities & Working from Home:

![](README_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

![](README_files/figure-gfm/unnamed-chunk-15-2.png)<!-- -->

The graphs show the relationship between productivity changes and
childcare responsibilities for employees working from home versus
on-site.

Among those working from home, individuals without childcare
responsibilities (2,440) report significantly more productivity changes
compared to those with childcare responsibilities (1,595). This suggests
that balancing childcare while working remotely may reduce the
likelihood of reported productivity changes.

For those not working from home, the overall numbers are lower, likely
because fewer employees worked on-site during the pandemic. However, a
similar trend is observed: individuals without childcare
responsibilities (592) report more productivity changes compared to
those with childcare responsibilities (395).

Overall, while fewer employees worked on-site, the data highlights that
balancing childcare responsibilities, especially in remote work
settings, may influence productivity.

##### Health Issues & Working from Home:

![](README_files/figure-gfm/unnamed-chunk-16-1.png)<!-- -->

Employees without health issues (Health Issue = 0) reported slightly
more productivity changes when working on-site (0.51) compared to remote
(0.50). In contrast, employees with health issues (Health Issue = 1)
reported fewer productivity changes on-site (0.49) compared to remote
work (0.51). This suggests that remote work environments may cause more
adjustments for employees with health challenges, while on-site work
remains relatively stable for this group.

#### Question 4: How does Stress_Level impact Productivity_Change when and when not working from Home?

![](README_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

The graph shows how stress levels and work arrangements relate to
productivity changes.

For employees with low stress, productivity changes are minimal and
nearly identical between remote work (0.501) and on-site work (0.51),
suggesting that their productivity remains consistent regardless of
location.

For those with medium stress, productivity changes are higher when
working remotely (0.507) compared to on-site work (0.484). This
indicates that remote work may introduce adjustments or flexibility that
result in more noticeable shifts in productivity.

In contrast, employees with high stress report fewer productivity
changes when working remotely (0.495) compared to on-site work (0.525).
This suggests that remote work helps stabilize productivity for
high-stress individuals by reducing disruptions.

Overall, remote work appears to offer stability for high-stress
employees, introduces greater adjustments for medium-stress individuals,
and has little impact on low-stress employees.

#### Question 5: Do employees who experienced salary cuts show different productivity trends compared to those with stable salaries?

    ## ### Analysis of Productivity Change by Salary Changes:
    ## - Average Productivity Change (Salary Cuts): 0.501
    ## - Average Productivity Change (Stable Salaries): 0.502
    ## - t-value: -0.1154 | p-value: 0.9081
    ## - 95% Confidence Interval: (-0.0262, 0.0233)
    ## **Conclusion**: There is no significant difference in productivity change between the two groups.

The analysis shows minimal differences in productivity trends between
employees who experienced salary cuts and those with stable salaries.

Employees with salary cuts have an average productivity change of 0.501.

Employees with stable salaries have an average productivity change of
0.502.

The median productivity change is 1 for both groups, suggesting that the
central tendency of productivity is identical.

Statistical Test:

A Welch Two-Sample t-test was conducted to compare the means of the two
groups:

The t-value is -0.1154 with a p-value of 0.9081, indicating no
statistically significant difference. The 95% confidence interval for
the difference in means is (-0.0262, 0.0233), further confirming the
lack of meaningful difference.

The data indicates that salary changes (cuts or stability) had
negligible impact on productivity trends during the observed period.
Both groups maintained similar productivity levels.

#### Question 6: What was the impact of Childcare Responsibilities, Commuting Changes, and Health Issues on Technology Adaptation Levels

![](README_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

![](README_files/figure-gfm/unnamed-chunk-19-2.png)<!-- -->

Employees without health issues had the highest technology adaptation
level of 0.626 when they had childcare responsibilities and no commuting
changes. When commuting changes were introduced, adaptation slightly
decreased to 0.606. Those without childcare responsibilities had lower
adaptation levels, with 0.591 for no commuting changes and 0.597 with
commuting changes.

For employees with health issues, the highest adaptation level of 0.628
was observed for those without childcare responsibilities and no
commuting changes. Adaptation for employees with childcare
responsibilities remained consistent at 0.615 (no commuting changes) and
0.616 (with commuting changes), indicating minimal impact of commuting
changes in this group.

Overall, technology adaptation peaked for employees without health
issues and with childcare responsibilities but dropped slightly when
commuting changes were introduced. For those with health issues,
commuting changes had less of an effect, and childcare responsibilities
showed stable adaptation levels. This suggests that employees managing
childcare were able to adapt well, particularly when health issues were
not a factor.

## Conclusion

#### Question 7: What are the key takeaways or recommendations based on our analysis?

The analysis highlights key insights into productivity, stress, and
adaptation during pandemic-like disruptions. Employees working from home
generally reported higher productivity changes, especially for medium
stress levels, while those with high stress experienced better stability
when working on-site. Childcare responsibilities significantly
influenced technology adaptation, with employees without health issues
showing the highest adaptation levels when managing childcare and no
commuting changes. Commuting changes had a marginal impact on employees
with health issues. Sectors like Education and IT reported more
productivity changes, indicating greater disruption, while Retail and
Healthcare demonstrated relative stability. Overall, recommendations
include providing targeted support for highly stressed employees,
enhancing resources for remote workers with childcare responsibilities,
and ensuring flexibility to balance collaboration and adaptation across
different sectors.

#### Question 8: What can we improve if this analysis were conducted again?

If this analysis were conducted again, improvements could include
gathering more detailed data on the nature of productivity changes to
distinguish between positive and negative impacts. Collecting
qualitative responses alongside quantitative data would provide deeper
insights into the reasons behind changes in stress levels and technology
adaptation. Addressing inconsistencies in binary variables, such as
clarifying their interpretation, would reduce ambiguity. Additionally,
incorporating time-based trends would allow us to analyze how
productivity, stress, and adaptation evolved over the course of the
pandemic. Finally, a larger and more diverse dataset across regions and
industries would improve the generalizability of the findings.
