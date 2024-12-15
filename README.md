
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

2.  What is the distribution of the Productivity_Change Variable?

3.  What is the impact of working from home on productivity change?

- Stress Level & Working from Home
- Sector
- Sector & Working from Home
- Childcare Responsibilities & Working from Home
- Health Issues & Working from Home

4.  Are there significant correlations between productivity_change and
    other numeric variables?

5.  Do employees who experienced salary cuts show different productivity
    trends compared to those with stable salaries?

6.  What was the impact of Childcare Responsibilities, Commuting
    Changes, and Health Issues on Technology Adaptation Levels

7.  What are the key takeaways or recommendations based on our analysis?

8.  What can we improve if this analysis were conducted again?

These are the primary questuons we seek to answer in this final project.
From the information drawn from these questions, we will be able to
derive meaningful insights on the best practices to use during Pandmeics
and better understand how to prepare for them.

## Data

### Structure

    The link to the data set is the following: https://www.kaggle.com/datasets/willianoliveiragibin/covid-19-on-working-professionals/data. The Kaggle website constains a CSV file containing all the data collected to detail the impact COVID had on people's work lives. There are 15 columns for 15 variables, where each variable describes a different aspect of work and whether or not it has had any change or effect from COVID. All of the data-points are neatly organized into one CSV file, where there are exactly 10,000 rows.

    One of the most important things to note about this dataset is that fact that most of the variables report their data in a binary format. In other words, most of the data is listed in 0's and 1's, where 0 conveys that the given data-point has not been effected or changed for its given variable/column while 1 conveys that it has. Although this may seem subtle, this has lead to a variety of issues and mis-interpretations throughout this project, which have been since fixed of course. 

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
data set experinced and impact or change to their job securities during
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

### Variables

Although we use most of these variables througout this analysis, our
variables of focus are going to be the “Productivity_Change” and
“Stress_Level” since these variables will tell us how society is doing
as a whole.

## Results

## Conclusion
