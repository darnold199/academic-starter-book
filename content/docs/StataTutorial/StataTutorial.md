---
title: STATA Tutorial
linktitle: STATA Tutorial
type: book
date: "2019-05-05T00:00:00+01:00"
# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

# Introduction to STATA

In this notebook I will be introducing STATA, a statistical software package that is very popular in many social sciences.

## The STATA Interface

When you open stata you will be greeted by the STATA interface:

<img src="img/statainterface2.png" width="800">

There are four separate windows in this interace that we will discuss in turn. We will start in the bottom left, in the **Command Window**. The command window is where you can enter commands into stata. The output of the command will then appear in the aptly named **Results Viewer**.

For example, a basic command in stata is "di" (short for display). This can be used as a calculator or to display text. For example if we write into the command window:


```stata
di "Hello World"
```

    Hello World


Then we see "Hello World" displayed in the results window. In general, you should **not** use the command window very often. It is much more efficient to type commands into a "do-file" which is a script that has a list of commands. You can then run the script file in order to execute the list of commands. This will allow you to quickly rerun commands and save you a lot of time retyping commands that were slightly wrong. It also allows others to reproduce your work. You can just send them the do-file!

The remaining two windows on the right are the **Variables Window** and the **Properties Window**. The variables window will display the name of the variables in a dataset along with a short description in Label (not everyone that constructs datasets adds labels to all variables, though it is good practice to do so. Also, in many cases you will be constructing your dataset from other sources and will need to add the labels yourself).

## Workflow 

Before loading the dataset, now is a good time to discuss workflow, which for our purposes will be how to organize your files. In this class, we will have a number of empirical problem sets, and you will save time and frustration simply by setting up your folders in an efficient way.

To understand what I mean about workflow, let's take an example of how my folders were probably organized for my first empirical problem set. 

<img src = "img/baddirectory1.png" width="600">

Ok, so there is a lot to dislike by this trainwreck of a folder. First, there are a number of different data sets, and the difference between them is not at all clear. Imagine taking a break from the problem set and coming back to this folder. You might waste a lot of time trying to figure out what file is useful for what. Is the right dataset data_clean.dta, data_new.dta or data.dta? What is data_alt.dta and temp_data.dta? Also, there are two folders "misc" and "temp" and it is unclear what makes a file or dataset miscellaneous vs. what makes a file or dataset temporary. 

In practie, for a single problem set, you can probably get by with this type of workflow. But as projects get more complicated, bad workflow practices will make your work more and more frustrating. It is better to stay organized (even if it takes a bit longer to set up at times). So we know this is a bad directory, but what does a good directory look like? 

First, lets add more folders that separate files by their function, as follows:

<img src = "img/gooddirectory2.png" width="600">

So now we have four different folders, each with a clear purpose. "code" will house all the code for the problem set. "data" will include all the data for the problem set. "results" will have the results from the problem set, such as tables and figures, along with the write up of the results. "archive" will be our folder where we put old versions of files we don't want to delete. While this gives us a good structure for the problem set, it will also be important to not let these folders get disorganized. Don't have multiple versions of code that are unclear in how they relate. Don't find yourself in a situation in which you don't know what version of the dataset you should be using. 

## Log Files, the Working Directory and Loading Data

All right, we have our folders setup and are ready to analyze some data. For this tutorial we are going to load in a dataset from the Researchers at [Opportunity Insights](https://opportunityinsights.org/). This is a truly impressive dataset that answers a particularly interesting question that would have been very difficult to answer even a few years ago. 

The question is the following: how does the higher education system shape intergenerational income inequality in the United States? While college is often seen as a pathway to higher earnings, if children from higher-income families attend better colleges on average, the college system as a whole may not promote mobility and could even amplify income inequality across generations.

To answer this question, the researchers link students to their parents using federal income tax returns. They then link students to their colleges. This allows them to study whether a college takes in a lot of students from low-income backgrounds and then produces high-income graduates. The datset we will be using has an observation for every college in the U.S., along with different measures of intergenerational mobility for its students.


### Log Files

Log files are records of your session in STATA. Everything you type and every result that comes out of the command window will be record in the log file. This can be helpful for a number of reasons, including keeping track of what you've done, viewing particularly long ouput, as well as sending to collaborators (or attaching as part of your problem set!)

To create a log file type:


```stata
log using tutorial_logfile, text replace
```

    (note: file /Users/davidarnold/Dropbox/Teaching/ECON5/Jupyter/code/tutorial_logf
    > ile.log not found)
    --------------------------------------------------------------------------------
          name:  <unnamed>
           log:  /Users/davidarnold/Dropbox/Teaching/ECON5/Jupyter/code/tutorial_log
    > file.log
      log type:  text
     opened on:  19 Aug 2020, 16:21:57



### The Working Directory

Before we get to analyzing the data, we first need to load it into STATA. This is where we will learn about the working directory. You can check your working directory from within STATA by typing:



```stata
pwd
```

    /Users/davidarnold/Dropbox/Teaching/ECON5/Jupyter/code


I can easily change my working directory at any time from within stata. For example, if I want to change the working directory to the results folder instead of the code folder:


```stata
cd /Users/davidarnold/Dropbox/Teaching/ECON5/Jupyter/results
```

    /Users/davidarnold/Dropbox/Teaching/ECON5/Jupyter/results


I can also change my directory back by using a shortcut:


```stata
cd ../code
```

    /Users/davidarnold/Dropbox/Teaching/ECON5/Jupyter/code


The ".." tells the computer to look back one folder (so look back to Jupyter) and then change the directory to "/code". In order to use this method you need to understand where your working directory is. If I tell the computer to change to a directory that doesn't exist I will just get an error.

I have placed a version of the dataset named "mrc_table1-2.dta" in my folder titled "data". Now to load the dataset we can use the ``use`` command:


```stata
use /Users/davidarnold/Dropbox/Teaching/ECON5/Jupyter/data/mrc_table1-2.dta, replace
```

    (Preferred Estimates of Access and Mobility Rates by College)


The ``,replace`` option tells STATA to replace whatever data is currently loaded. If there is no data loaded, then you don't strictly need to put the ``,replace`` option. 

One thing to point out here is that I did not have to actually write out the entire path. I just need to understand where the data is relative to my working directory. Therefore, as better way to load the data would be to type:



```stata
use ../data/mrc_table1-2.dta, replace
```

    (Preferred Estimates of Access and Mobility Rates by College)


And again I load the data. 

## Getting Started -- Help files, describing, and summarizing data

Ok, so now we have the data loaded. What next? The first thing to do is to get a sense of what is in the data. A good way to is to use the ``describe`` command


```stata
describe 
```

    
    Contains data from ../data/mrc_table1-2.dta
      obs:         2,202                          Preferred Estimates of Access
                                                    and Mobility Rates by College
     vars:            15                          21 Jul 2017 22:57
    --------------------------------------------------------------------------------
                  storage   display    value
    variable name   type    format     label      variable label
    --------------------------------------------------------------------------------
    super_opeid     double  %12.0g                Institution OPEID / Cluster ID
                                                    when combining multiple OPEIDs
    name            str141  %141s                 Name of Institution / Super-OPEID
                                                    Cluster
    czname          str22   %22s                  Commuting Zone Name
    state           str2    %9s                   State of constituent instition
                                                    with highest enrollment
    par_median      double  %9.0g                 Median parental income
    k_median        float   %9.0g                 Median kid income
    par_q1          float   %9.0g                 Fraction of parents in quintile 1
                                                    (bottom quintile)
    par_top1pc      float   %9.0g                 Fraction of parents in the top 1%
    kq5_cond_parq1  float   %9.0g                 Probability of kid in quintile 5
                                                    conditional on parent in
                                                    quintile 1
    ktop1pc_cond_~1 float   %9.0g                 Probability of kid in top 1%
                                                    conditional on parent in
                                                    quintile
    mr_kq5_pq1      float   %9.0g                 Mobility rate
    mr_ktop1_pq1    float   %9.0g                 Tail mobility rate
    trend_parq1     float   %9.0g                 Trend in Share of Kids from Bottom
                                                    20%
    trend_bottom40  float   %9.0g                 Trend in Share of Kids from Bottom
                                                    40%
    count           double  %12.0g                Mean number of kids per cohort
    --------------------------------------------------------------------------------
    Sorted by: 


So the ``describe`` command lists some basic statistics about the datset. This dataset has 2,202 observations (or colleges in this case) and 15 variables. In the first collumn we have the variable name. In the second column we have the storage type. "double" and "float" are both numeric variables while the variables with "str" as the prefix are text variables (generally referred to as string variables in stata). Lastly, we have the variable label which gives us a short description of each variable.

Before getting started, it is (sometimes) helpful to just look at the raw data. We can achieve this with typing ``browse`` into command line. Because I don't want to inundate you with a lot of data. I will just show you a small snapshot of the data: 


```stata
%head
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>super_opeid</th>
      <th>name</th>
      <th>czname</th>
      <th>state</th>
      <th>par_median</th>
      <th>k_median</th>
      <th>par_q1</th>
      <th>par_top1pc</th>
      <th>kq5_cond_parq1</th>
      <th>ktop1pc_cond_parq1</th>
      <th>mr_kq5_pq1</th>
      <th>mr_ktop1_pq1</th>
      <th>trend_parq1</th>
      <th>trend_bottom40</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2665</td>
      <td>Vaughn College Of Aeronautics And Technology</td>
      <td>New York</td>
      <td>NY</td>
      <td>30900</td>
      <td>53000</td>
      <td>.36477882</td>
      <td>.0011981524</td>
      <td>.44843543</td>
      <td>.017666299</td>
      <td>.16357975</td>
      <td>.0064442917</td>
      <td>-.079987764</td>
      <td>-.057506107</td>
      <td>207.6666666666667</td>
    </tr>
    <tr>
      <th>2</th>
      <td>7273</td>
      <td>CUNY Bernard M. Baruch College</td>
      <td>New York</td>
      <td>NY</td>
      <td>42800</td>
      <td>57600</td>
      <td>.27632242</td>
      <td>.0055920202</td>
      <td>.46824235</td>
      <td>.025568271</td>
      <td>.12938586</td>
      <td>.0070650866</td>
      <td>-.091865495</td>
      <td>-.12297224</td>
      <td>1083</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2688</td>
      <td>City College Of New York - CUNY</td>
      <td>New York</td>
      <td>NY</td>
      <td>35500</td>
      <td>48500</td>
      <td>.32546476</td>
      <td>.0023351549</td>
      <td>.36021557</td>
      <td>.014087214</td>
      <td>.11723747</td>
      <td>.0045848917</td>
      <td>-.0980158</td>
      <td>-.13879366</td>
      <td>582.3333333333334</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7022</td>
      <td>CUNY Lehman College</td>
      <td>New York</td>
      <td>NY</td>
      <td>32500</td>
      <td>40700</td>
      <td>.36707491</td>
      <td>0</td>
      <td>.27882966</td>
      <td>.0018963498</td>
      <td>.10235137</td>
      <td>.00069610245</td>
      <td>-.057339657</td>
      <td>-.090723462</td>
      <td>468.3333333333333</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1140</td>
      <td>California State University, Los Angeles</td>
      <td>Los Angeles</td>
      <td>CA</td>
      <td>36600</td>
      <td>43000</td>
      <td>.33116928</td>
      <td>.0015598091</td>
      <td>.29949805</td>
      <td>.00083619979</td>
      <td>.09918455</td>
      <td>.00027692367</td>
      <td>-.13313572</td>
      <td>-.14919846</td>
      <td>1179.666666666667</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2693</td>
      <td>CUNY John Jay College Of Criminal Justice</td>
      <td>New York</td>
      <td>NY</td>
      <td>41800</td>
      <td>45200</td>
      <td>.27158952</td>
      <td>.00097911153</td>
      <td>.35684139</td>
      <td>.0030840749</td>
      <td>.096914381</td>
      <td>.00083760242</td>
      <td>-.039944887</td>
      <td>-.081856675</td>
      <td>1228</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2165</td>
      <td>MCPHS University</td>
      <td>Boston</td>
      <td>MA</td>
      <td>83300</td>
      <td>112700</td>
      <td>.10234574</td>
      <td>.0077554816</td>
      <td>.91293561</td>
      <td>.094175987</td>
      <td>.093435071</td>
      <td>.0096385116</td>
      <td>-.015396656</td>
      <td>-.05452856</td>
      <td>173.5</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2791</td>
      <td>Pace University</td>
      <td>New York</td>
      <td>NY</td>
      <td>68600</td>
      <td>60700</td>
      <td>.15173292</td>
      <td>.012134688</td>
      <td>.55575591</td>
      <td>.027615389</td>
      <td>.084326468</td>
      <td>.0041901637</td>
      <td>-.059509847</td>
      <td>-.13538049</td>
      <td>1353.333333333333</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2838</td>
      <td>State University Of New York At Stony Brook</td>
      <td>New York</td>
      <td>NY</td>
      <td>73600</td>
      <td>60100</td>
      <td>.16416624</td>
      <td>.0044201165</td>
      <td>.51245296</td>
      <td>.019444995</td>
      <td>.084127478</td>
      <td>.0031922117</td>
      <td>-.067162491</td>
      <td>-.10386624</td>
      <td>2070.666666666667</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2696</td>
      <td>New York City College Of Technology Of The Cit...</td>
      <td>New York</td>
      <td>NY</td>
      <td>33500</td>
      <td>37000</td>
      <td>.35256514</td>
      <td>.0010819947</td>
      <td>.23638399</td>
      <td>.0019894305</td>
      <td>.083340757</td>
      <td>.00070140383</td>
      <td>-.070691638</td>
      <td>-.091352046</td>
      <td>1488.333333333333</td>
    </tr>
  </tbody>
</table>
</div>


Ok so now we want to dig a bit deeper into some of these variables. Let's look at par_median, which as we know from the ``describe`` command is the median parental income for a given college. Let's compute some simple summary statistics by using the ``summarize`` command:
    


```stata
summarize par_median
```

    
        Variable |        Obs        Mean    Std. Dev.       Min        Max
    -------------+---------------------------------------------------------
      par_median |      2,202    77695.46    28463.28      21200     226700


Ok, so the average median parental income for a college student in this sample is around 78,000 dollars. However, there is considerable spread across colleges. The lowest is 21200 and the highest is 226,700. It seems helpful now to get a better sense of that variation between those minimum and maximum values. Let's see if the ``summarize`` command can help us somehow. Many (most) commands have additional options and it is very easy to look up those options. ``help`` [command name] into the command line:



```stata
help summarize
```

    
    [R] summarize -- Summary statistics
                     (View complete PDF manual entry)
    
    
    Syntax
    ------
    
            summarize [varlist] [if] [in] [weight] [, options]
    
        options           Description
        --------------------------------------------------------------------------
        Main
          detail          display additional statistics
          meanonly        suppress the display; calculate only the mean;
                            programmer's option
          format          use variable's display format
          separator(#)    draw separator line after every # variables; default is
                            separator(5)
          display_options control spacing, line width, and base and empty cells
    
        --------------------------------------------------------------------------
        varlist may contain factor variables; see fvvarlist.
        varlist may contain time-series operators; see tsvarlist.
        by, rolling, and statsby are allowed; see prefix.
      
        aweights, fweights, and iweights are allowed.  However, iweights may not
          be used with the detail option; see weight.
    
    
    Menu
    ----
    
        Statistics > Summaries, tables, and tests > Summary and descriptive
            statistics > Summary statistics
    
    
    Description
    -----------
    
        summarize calculates and displays a variety of univariate summary
        statistics.  If no varlist is specified, summary statistics are calculated
        for all the variables in the dataset.
    
    
    Links to PDF documentation
    --------------------------
    
            Quick start
    
            Remarks and examples
    
            Methods and formulas
    
        The above sections are not included in this help file.
    
    
    Options
    -------
    
            +------+
        ----+ Main +--------------------------------------------------------------
    
        detail produces additional statistics, including skewness, kurtosis, the
            four smallest and four largest values, and various percentiles.
    
        meanonly, which is allowed only when detail is not specified, suppresses
            the display of results and calculation of the variance.  Ado-file
            writers will find this useful for fast calls.
    
        format requests that the summary statistics be displayed using the display
            formats associated with the variables rather than the default g
            display format; see [D] format.
    
        separator(#) specifies how often to insert separation lines into the
            output.  The default is separator(5), meaning that a line is drawn
            after every five variables.  separator(10) would draw a line after
            every 10 variables.  separator(0) suppresses the separation line.
    
        display_options:  vsquish, noemptycells, baselevels, allbaselevels,
            nofvlabel, fvwrap(#), and fvwrapon(style); see [R] Estimation options.
    
    
    Examples
    --------
    
        . sysuse auto
        . summarize
        . summarize mpg weight
        . summarize mpg weight if foreign
        . summarize mpg weight if foreign, detail
        . summarize i.rep78
    
    
    Video example
    -------------
    
        Descriptive statistics in Stata
    --more--
    


There are few very helpful parts of this help file. Firs, we can see the syntax for the command. When we first learn a new command, it is common to get a number of erros. Looking carefully at the syntax is usually a clue about where we are going wrong. For ``summarize`` it expects a list of variables (that is what [varlist] refers to). You can also do conditional ``summarize`` commands (that is the [if] [in] refers to, we will do an example of this below). It appears that there are also options available, that are helpfully listed right below the syntax. If we want more details and examples we can just scroll down (if you are ever confused about what the syntax means exactly, it is often helpful to scroll down to the examples). 

Ok, so for our purposes we wanted to know more about the distribution of median parental income, so let's give ``detail`` a shot. The description of this option said it displayed various percentiles so that sounds like it will be helpful here.


```stata
summarize par_median, detail
```

    
                       Median parental income
    -------------------------------------------------------------
          Percentiles      Smallest
     1%        29300          21200
     5%        37800          23900
    10%        45100          25100       Obs               2,202
    25%        59100          26400       Sum of Wgt.       2,202
    
    50%        74300                      Mean           77695.46
                            Largest       Std. Dev.      28463.28
    75%        91700         208900
    90%       110300         218100       Variance       8.10e+08
    95%       128200         219600       Skewness       1.222058
    99%       176900         226700       Kurtosis       5.948575


So now we have a better sense of the value of different percentiles on the left. At the bottom of the distribution moving from the 1st to the 5th percentile is associated with about an 8,500 dollar increase in income. At the top, going from 95th to 99th is associated with a 48,700 dollar increase in income. 

## Generating New Variables

Now that we have a sense of the data, let's get back to 
the primary question: how do colleges contribute to intergenerational income mobility? In particular, we will be focused on understanding which colleges are particularly effective at promoting intergenerational income mobility. Our preferred measure of mobility (defined as "mobility rate") will be a function of two parameters: 

$$
\begin{equation}
\text{mobility rate} = \text{fraction of students from bottom quintile} \times \text{fraction bottom that reach top quintile}
\end{equation}
$$



This definition comes from the paper this dataset is based on and is not self-contained in this dataset, so do not worry if this definition seemed to come out of nowhere. You would need to have read the paper to have determined this. 

So to explain this a bit further, for each college we need to compute two statistics. First, what fraction of students are low-income (which here we define as having parents below the 20th percentile of the income distribution). Second, what fraction of these low-income students achieve high income (i.e. defined as having income above the 80th percentile of income). In simple terms, a college that promotes integenerational mobility (i.e. has a high mobility rate) will tend to enroll more low-income students and the low-income students they do enroll will tend to have higher incomes.

In this, the variable labels are detailed enough that reading the our describe command we can quickly see that all of these variables are already included in the dataset. "mobility rate" is given by the variable ``mr_kq5_pq1``, "fraction of students from bottom quintile" is given by the variable ``par_q1``, and "fraction bottom that reach top quintile" is given by ``kq5_cond_parq1``.

But imagine we did not have the mobility rate variable in the data, but instead had only ``par_q1`` and ``kq5_cond_parq1``. Using the ``gen`` command we can easily construct the mobility rate variable.



```stata
gen mobility_rate = par_q1*kq5_cond_parq1
```

If we have correctly generated ``mobility_rate`` and correctly interpreted ``mr_kq5_pq1`` then the values of these two variables should be exactly the same. One easy way to check this is to use the ``count`` command. Using this command we can count the number of times that ``mobility_rate`` does not equal ``mr_kq5_pq1``



```stata
count if mobility_rate != mr_kq5_pq1
```

      0


The ``!=`` in the line above stands for "not equal to". As promised, the number of times the two are not equal is 0, meaning the two variables are always the same value. We can also count the number of times they are the same, which will just be the number of observations in the dataset:


```stata
count if mobility_rate == mr_kq5_pq1
```

      2,202


In order to explore the data a bit more we are going to create some new regional variables. We have the variable ``state`` in the data. We are going to create a binary variable that is equal to one if the college is in California, and zero otherwise. A binary version, in general, are variables that only take two values (in this case 0 if the college is in California and 1 if the college is in California).


```stata
gen CA = (state=="CA")
```

A useful way to describe binary (or categorical) variables is to use the ``tab`` command, which counts the number of observations in each cell defined by the binary variable. For example:


```stata
tab CA
```

    
             CA |      Freq.     Percent        Cum.
    ------------+-----------------------------------
              0 |      2,034       92.37       92.37
              1 |        168        7.63      100.00
    ------------+-----------------------------------
          Total |      2,202      100.00


So 168 of the colleges in the dataset are in California. Let's create another binary variable that is equal to one if the parental median income is greater than average parental median income across all colleges, which if you recall from ``summarize`` command was equal to 77,695.46. 


```stata
gen high_parent_income = (par_median>77695.46)
```

So now we have two binary variables. One that indicates high_parent_income and one that indicates whether the college is in CA. We can also use tabulate with both binary variables. In this case, the command will count the number of observations defined by the two binary variables, as we see below:


```stata
tab CA high_parent_income
```

    
               |  high_parent_income
            CA |         0          1 |     Total
    -----------+----------------------+----------
             0 |     1,138        896 |     2,034 
             1 |        88         80 |       168 
    -----------+----------------------+----------
         Total |     1,226        976 |     2,202 


So how do we interpret this table? The cell in the upper right-hand cornered, defined by (0,0), reports the number of colleges in the sample that have ``CA==0`` and ``high_parent_income==0``. So in total there are 1,138 colleges that are not in California, where the median parental income is below the nationwide average. The cell in the upper right-hand corner counts the number of colleges in the sample that have ``CA==0`` and ``high_parent_income==1``. So for colleges not in California, 896 have a median parental income above the nationwide average. So for non-CA colleges about 44 percent have median parental income above the national average.

The cell in the bottome left-hand corner, defined by (1,0), reports the number of colleges in the sample that have ``CA==1`` and ``high_parent_income==0``. So in total there are 88 colleges that are in California, where the median parental income is below the nationwide average. The cell in the bottom right-hand corner counts the number of colleges in the sample that have ``CA==1`` and ``high_parent_income==1``. So for colleges in California, 80 colleges have a median parental income above the nationwide average. So for CA colleges about 48 percent have median parental income above the national average.

Now let's get back to looking at mobility rates, and see how those vary across CA vs. other states. We can do this using a conditional ``summarize`` command. 



```stata
sum mobility_rate par_q1 kq5_cond_parq1 if CA==1
sum mobility_rate par_q1 kq5_cond_parq1 if CA==0
```

    
    
        Variable |        Obs        Mean    Std. Dev.       Min        Max
    -------------+---------------------------------------------------------
    mobility_r~e |        168    .0275095    .0154819          0   .0991846
          par_q1 |        168    .1443805    .0889914   .0321324   .4606968
    kq5_cond_p~1 |        168    .2481358    .1638556          0   .8497473
    
    
        Variable |        Obs        Mean    Std. Dev.       Min        Max
    -------------+---------------------------------------------------------
    mobility_r~e |      2,034     .017511    .0126387          0   .1635797
          par_q1 |      2,034    .1234973    .0880145   .0111896   .6097748
    kq5_cond_p~1 |      2,034    .1917753    .1360376          0   .9192932


So overall, the mobility rate is higher CA than it is for the rest of the country (0.027 vs 0.017). Recalling from our definition of mobility, we can also see that this is due to two factors. First, CA colleges tend to enroll more students from low-income backgrounds (the percent from low-income backgrounds in CA is 14.4 percent vs. 12.3 percent in other states). Additionally, the students from low-income backgrounds tend to earn more. 24.8 percent of the low-income students in CA go on to earn above the 80th percentile of income while 19.2 percent of low-income students from other states go on to earn above the 80th percentile of income. We can drill down even further and look at the data for UCSD alone.


```stata
sum mr_kq5_pq1 par_q1 kq5_cond_parq1 if name == "University Of California, San Diego"
```

    
        Variable |        Obs        Mean    Std. Dev.       Min        Max
    -------------+---------------------------------------------------------
      mr_kq5_pq1 |          1    .0483275           .   .0483275   .0483275
          par_q1 |          1    .0877724           .   .0877724   .0877724
    kq5_cond_p~1 |          1    .5506001           .   .5506001   .5506001


So for UCSD, about 9 percent of students come from parents in the bottom 20th percentile of the income distribution, which was lower than the average for all of CA. However, of those 9 percent, 55 percent become high-earners. Therefore, overall, UCSD looks to have a high mobility rate compared to other schools in the nation, as well as other states in CA.

## Graphing in Stata

In this class we are going to put a lot of emphasis on data visualization. It is important to clearly convey the information from the data. Luckily, stata has excellent graphics packages that will help us here. To understand the capabilities, let's look at the help file:








```stata
help graph
```

    
    [G-2] graph -- The graph command
                   (View complete PDF manual entry)
    
    
    Syntax
    ------
    
            graph ...
    
        The commands that draw graphs are
    
            Command                 Description
            ----------------------------------------------------------------------
            graph twoway            scatterplots, line plots, etc.
            graph matrix            scatterplot matrices
            graph bar               bar charts
            graph dot               dot charts
            graph box               box-and-whisker plots
            graph pie               pie charts
            other                   more commands to draw statistical graphs
            ----------------------------------------------------------------------
    
        The commands that save a previously drawn graph, redisplay previously
        saved graphs, and combine graphs are
    
            Command                 Description
            ----------------------------------------------------------------------
            graph save              save graph to disk
            graph use               redisplay graph stored on disk
            graph display           redisplay graph stored in memory
            graph combine           combine multiple graphs
            graph replay            redisplay graphs stored in memory and on disk
            ----------------------------------------------------------------------
    
        The commands for printing a graph are
    
            Command                 Description
            ----------------------------------------------------------------------
            graph print             print currently displayed graph
            set printcolor          set how colors are printed
            graph export            export .gph file to PostScript, etc.
            ----------------------------------------------------------------------
    
        The commands that deal with the graphs currently stored in memory are
    
            Command                 Description
            ----------------------------------------------------------------------
            graph display           display graph
            graph dir               list names
            graph describe          describe contents
            graph rename            rename memory graph
            graph copy              copy memory graph to new name
            graph drop              discard graphs in memory
            graph close             close Graph windows
            ----------------------------------------------------------------------
            Also see [G-2] graph manipulation.
    
        The commands that describe available schemes and allow you to identify and
        set the default scheme are
    
            Command                 Description
            ----------------------------------------------------------------------
            graph query, schemes    list available schemes
            query graphics          identify default scheme
            set scheme              set default scheme
            ----------------------------------------------------------------------
            Also see [G-4] Schemes intro.
    
        The command that lists available styles is
    
            Command                 Description
            ----------------------------------------------------------------------
            graph query             list available styles
            ----------------------------------------------------------------------
    
        The command for setting options for printing and exporting graphs is
    
            Command                 Description
            ----------------------------------------------------------------------
            graph set               set graphics options
            ----------------------------------------------------------------------
    
        The command that allows you to draw graphs without displaying them is
    
            Command                 Description
            ----------------------------------------------------------------------
            set graphics            set whether graphs are displayed
            ----------------------------------------------------------------------
    
    
    Description
    -----------
    
        graph draws graphs.
    
    
    Links to PDF documentation
    --------------------------
    --more--


The graph command encompasses a large number of commands. For our purposes, we will generally be using ``graph twoway`` which includes a large number of different plot types. Going back to our main example, we think it would be interesting to understand how much variability there is in mobility rates across colleges. We've already looked at CA vs other states, as well as UCSD in particular, but we would like to get a sense of all the data. 

Recalling from class, one way to approximate the distribution of a numerical variable is to construct a histogram. This entails cutting up our numerical variable (mobility rates in this case) into a number of different bins, and then counting the fraction of observations that fall within each bin. Let's try it out


```stata
graph twoway histogram mobility_rate
```


                <iframe frameborder="0" scrolling="no" height="436" width="600"                srcdoc="<html><body>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
&lt;!-- This is a Stata 16.1 generated SVG file (http://www.stata.com) --&gt;

&lt;svg version=&quot;1.1&quot; width=&quot;600px&quot; height=&quot;436px&quot; viewBox=&quot;0 0 3960 2880&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot; xmlns:xlink=&quot;http://www.w3.org/1999/xlink&quot;&gt;
	&lt;desc&gt;Stata Graph - Graph&lt;/desc&gt;
	&lt;rect x=&quot;0&quot; y=&quot;0&quot; width=&quot;3960&quot; height=&quot;2880&quot; style=&quot;fill:#EAF2F3;stroke:none&quot;/&gt;
	&lt;rect x=&quot;0.00&quot; y=&quot;0.00&quot; width=&quot;3959.88&quot; height=&quot;2880.00&quot; style=&quot;fill:#EAF2F3&quot;/&gt;
	&lt;rect x=&quot;2.88&quot; y=&quot;2.88&quot; width=&quot;3954.12&quot; height=&quot;2874.24&quot; style=&quot;fill:none;stroke:#EAF2F3;stroke-width:5.76&quot;/&gt;
	&lt;rect x=&quot;390.80&quot; y=&quot;100.86&quot; width=&quot;3468.22&quot; height=&quot;2388.33&quot; style=&quot;fill:#FFFFFF&quot;/&gt;
	&lt;rect x=&quot;393.68&quot; y=&quot;103.74&quot; width=&quot;3462.46&quot; height=&quot;2382.57&quot; style=&quot;fill:none;stroke:#FFFFFF;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2425.82&quot; x2=&quot;3859.02&quot; y2=&quot;2425.82&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1990.46&quot; x2=&quot;3859.02&quot; y2=&quot;1990.46&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1555.09&quot; x2=&quot;3859.02&quot; y2=&quot;1555.09&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1119.72&quot; x2=&quot;3859.02&quot; y2=&quot;1119.72&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;684.24&quot; x2=&quot;3859.02&quot; y2=&quot;684.24&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;248.87&quot; x2=&quot;3859.02&quot; y2=&quot;248.87&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;454.16&quot; y=&quot;2054.93&quot; width=&quot;101.23&quot; height=&quot;370.89&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;458.48&quot; y=&quot;2059.25&quot; width=&quot;92.59&quot; height=&quot;362.25&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;555.39&quot; y=&quot;794.38&quot; width=&quot;101.23&quot; height=&quot;1631.45&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;559.71&quot; y=&quot;798.70&quot; width=&quot;92.59&quot; height=&quot;1622.81&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;656.62&quot; y=&quot;164.22&quot; width=&quot;101.35&quot; height=&quot;2261.60&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;660.94&quot; y=&quot;168.54&quot; width=&quot;92.71&quot; height=&quot;2252.96&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;757.97&quot; y=&quot;622.85&quot; width=&quot;101.23&quot; height=&quot;1802.97&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;762.29&quot; y=&quot;627.17&quot; width=&quot;92.59&quot; height=&quot;1794.33&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;859.20&quot; y=&quot;1420.69&quot; width=&quot;101.23&quot; height=&quot;1005.13&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;863.52&quot; y=&quot;1425.01&quot; width=&quot;92.59&quot; height=&quot;996.49&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;960.42&quot; y=&quot;1831.56&quot; width=&quot;101.23&quot; height=&quot;594.27&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;964.74&quot; y=&quot;1835.88&quot; width=&quot;92.59&quot; height=&quot;585.63&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1061.65&quot; y=&quot;2011.00&quot; width=&quot;101.35&quot; height=&quot;414.82&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1065.97&quot; y=&quot;2015.32&quot; width=&quot;92.71&quot; height=&quot;406.18&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1163.00&quot; y=&quot;2202.45&quot; width=&quot;101.23&quot; height=&quot;223.38&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1167.32&quot; y=&quot;2206.77&quot; width=&quot;92.59&quot; height=&quot;214.74&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1264.23&quot; y=&quot;2302.19&quot; width=&quot;101.23&quot; height=&quot;123.63&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1268.55&quot; y=&quot;2306.51&quot; width=&quot;92.59&quot; height=&quot;114.99&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1365.46&quot; y=&quot;2350.09&quot; width=&quot;101.23&quot; height=&quot;75.74&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1369.78&quot; y=&quot;2354.41&quot; width=&quot;92.59&quot; height=&quot;67.10&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1466.68&quot; y=&quot;2361.97&quot; width=&quot;101.35&quot; height=&quot;63.86&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1471.00&quot; y=&quot;2366.29&quot; width=&quot;92.71&quot; height=&quot;55.22&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1568.04&quot; y=&quot;2377.93&quot; width=&quot;101.23&quot; height=&quot;47.89&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1572.36&quot; y=&quot;2382.25&quot; width=&quot;92.59&quot; height=&quot;39.25&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1669.26&quot; y=&quot;2393.89&quot; width=&quot;101.23&quot; height=&quot;31.93&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1673.58&quot; y=&quot;2398.21&quot; width=&quot;92.59&quot; height=&quot;23.29&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1770.49&quot; y=&quot;2373.97&quot; width=&quot;101.23&quot; height=&quot;51.85&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1774.81&quot; y=&quot;2378.29&quot; width=&quot;92.59&quot; height=&quot;43.21&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1871.72&quot; y=&quot;2409.86&quot; width=&quot;101.35&quot; height=&quot;15.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1876.04&quot; y=&quot;2414.18&quot; width=&quot;92.71&quot; height=&quot;7.32&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1973.07&quot; y=&quot;2413.82&quot; width=&quot;101.23&quot; height=&quot;12.00&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1977.39&quot; y=&quot;2418.14&quot; width=&quot;92.59&quot; height=&quot;3.36&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2074.30&quot; y=&quot;2401.94&quot; width=&quot;101.23&quot; height=&quot;23.88&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2078.62&quot; y=&quot;2406.26&quot; width=&quot;92.59&quot; height=&quot;15.24&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2175.53&quot; y=&quot;2421.86&quot; width=&quot;101.35&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2179.84&quot; y=&quot;2421.50&quot; width=&quot;92.71&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2276.88&quot; y=&quot;2421.86&quot; width=&quot;101.23&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2281.20&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2378.10&quot; y=&quot;2421.86&quot; width=&quot;101.23&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2382.42&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2479.33&quot; y=&quot;2417.90&quot; width=&quot;101.23&quot; height=&quot;7.92&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2483.65&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;0.72&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2783.14&quot; y=&quot;2421.86&quot; width=&quot;101.23&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2787.46&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3086.94&quot; y=&quot;2421.86&quot; width=&quot;101.23&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;3091.26&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3694.43&quot; y=&quot;2421.86&quot; width=&quot;101.23&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;3698.75&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;390.80&quot; y2=&quot;100.86&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2425.82&quot; x2=&quot;350.83&quot; y2=&quot;2425.82&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;2425.82&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,2425.82)&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1990.46&quot; x2=&quot;350.83&quot; y2=&quot;1990.46&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1990.46&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1990.46)&quot; text-anchor=&quot;middle&quot;&gt;10&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1555.09&quot; x2=&quot;350.83&quot; y2=&quot;1555.09&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1555.09&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1555.09)&quot; text-anchor=&quot;middle&quot;&gt;20&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1119.72&quot; x2=&quot;350.83&quot; y2=&quot;1119.72&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1119.72&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1119.72)&quot; text-anchor=&quot;middle&quot;&gt;30&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;684.24&quot; x2=&quot;350.83&quot; y2=&quot;684.24&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;684.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,684.24)&quot; text-anchor=&quot;middle&quot;&gt;40&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;248.87&quot; x2=&quot;350.83&quot; y2=&quot;248.87&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;248.87&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,248.87)&quot; text-anchor=&quot;middle&quot;&gt;50&lt;/text&gt;
	&lt;text x=&quot;190.71&quot; y=&quot;1294.96&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 190.71,1294.96)&quot; text-anchor=&quot;middle&quot;&gt;Density&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;3859.02&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;454.16&quot; y1=&quot;2489.19&quot; x2=&quot;454.16&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;454.16&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;1475.47&quot; y1=&quot;2489.19&quot; x2=&quot;1475.47&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1475.47&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.05&lt;/text&gt;
	&lt;line x1=&quot;2496.90&quot; y1=&quot;2489.19&quot; x2=&quot;2496.90&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2496.90&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.1&lt;/text&gt;
	&lt;line x1=&quot;3518.34&quot; y1=&quot;2489.19&quot; x2=&quot;3518.34&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3518.34&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.15&lt;/text&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2729.16&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;mobility_rate&lt;/text&gt;
&lt;/svg&gt;
</body></html>"></iframe>



Ok, so interestingly mobility seems to have a "long right tail". While most mobility rates cluster in the 0.02-0.04 range, we have some colleges that have extremely high mobility rates, with some observations above 0.15. There are a number of different modifications we can now make as we seek to understand the variation in mobility rates across college.

First, stata has used a rule-of-thumb to construct these bins. Maybe we want to specify ourselves how large these bins are and see how the picture changes. If we type ``help histogram`` we could see all the different options we have here. One is ``width()`` which allows us to specify the width of each bin. 


```stata
graph twoway histogram mobility_rate, width(0.001)
```


                <iframe frameborder="0" scrolling="no" height="436" width="600"                srcdoc="<html><body>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
&lt;!-- This is a Stata 16.1 generated SVG file (http://www.stata.com) --&gt;

&lt;svg version=&quot;1.1&quot; width=&quot;600px&quot; height=&quot;436px&quot; viewBox=&quot;0 0 3960 2880&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot; xmlns:xlink=&quot;http://www.w3.org/1999/xlink&quot;&gt;
	&lt;desc&gt;Stata Graph - Graph&lt;/desc&gt;
	&lt;rect x=&quot;0&quot; y=&quot;0&quot; width=&quot;3960&quot; height=&quot;2880&quot; style=&quot;fill:#EAF2F3;stroke:none&quot;/&gt;
	&lt;rect x=&quot;0.00&quot; y=&quot;0.00&quot; width=&quot;3959.88&quot; height=&quot;2880.00&quot; style=&quot;fill:#EAF2F3&quot;/&gt;
	&lt;rect x=&quot;2.88&quot; y=&quot;2.88&quot; width=&quot;3954.12&quot; height=&quot;2874.24&quot; style=&quot;fill:none;stroke:#EAF2F3;stroke-width:5.76&quot;/&gt;
	&lt;rect x=&quot;390.80&quot; y=&quot;100.86&quot; width=&quot;3468.22&quot; height=&quot;2388.33&quot; style=&quot;fill:#FFFFFF&quot;/&gt;
	&lt;rect x=&quot;393.68&quot; y=&quot;103.74&quot; width=&quot;3462.46&quot; height=&quot;2382.57&quot; style=&quot;fill:none;stroke:#FFFFFF;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2425.82&quot; x2=&quot;3859.02&quot; y2=&quot;2425.82&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1671.91&quot; x2=&quot;3859.02&quot; y2=&quot;1671.91&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;918.01&quot; x2=&quot;3859.02&quot; y2=&quot;918.01&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;164.22&quot; x2=&quot;3859.02&quot; y2=&quot;164.22&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;454.16&quot; y=&quot;1946.52&quot; width=&quot;16.71&quot; height=&quot;479.30&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;458.48&quot; y=&quot;1950.84&quot; width=&quot;8.07&quot; height=&quot;470.66&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;470.87&quot; y=&quot;2271.75&quot; width=&quot;16.71&quot; height=&quot;154.07&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;475.19&quot; y=&quot;2276.07&quot; width=&quot;8.07&quot; height=&quot;145.43&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;487.57&quot; y=&quot;2254.67&quot; width=&quot;16.71&quot; height=&quot;171.15&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;491.89&quot; y=&quot;2258.99&quot; width=&quot;8.07&quot; height=&quot;162.51&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;504.28&quot; y=&quot;2117.68&quot; width=&quot;16.71&quot; height=&quot;308.15&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;508.60&quot; y=&quot;2122.00&quot; width=&quot;8.07&quot; height=&quot;299.51&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;520.99&quot; y=&quot;1929.45&quot; width=&quot;16.71&quot; height=&quot;496.38&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;525.31&quot; y=&quot;1933.77&quot; width=&quot;8.07&quot; height=&quot;487.74&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;537.69&quot; y=&quot;1638.38&quot; width=&quot;16.71&quot; height=&quot;787.45&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;542.01&quot; y=&quot;1642.70&quot; width=&quot;8.07&quot; height=&quot;778.81&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;554.40&quot; y=&quot;1278.87&quot; width=&quot;16.71&quot; height=&quot;1146.95&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;558.72&quot; y=&quot;1283.19&quot; width=&quot;8.07&quot; height=&quot;1138.31&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;571.11&quot; y=&quot;1090.64&quot; width=&quot;16.71&quot; height=&quot;1335.18&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;575.43&quot; y=&quot;1094.96&quot; width=&quot;8.07&quot; height=&quot;1326.54&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;587.81&quot; y=&quot;645.50&quot; width=&quot;16.71&quot; height=&quot;1780.32&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;592.13&quot; y=&quot;649.82&quot; width=&quot;8.07&quot; height=&quot;1771.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;604.52&quot; y=&quot;268.92&quot; width=&quot;16.71&quot; height=&quot;2156.91&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;608.84&quot; y=&quot;273.24&quot; width=&quot;8.07&quot; height=&quot;2148.27&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;621.23&quot; y=&quot;525.71&quot; width=&quot;16.71&quot; height=&quot;1900.12&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;625.54&quot; y=&quot;530.03&quot; width=&quot;8.07&quot; height=&quot;1891.48&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;637.93&quot; y=&quot;679.78&quot; width=&quot;16.71&quot; height=&quot;1746.04&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;642.25&quot; y=&quot;684.10&quot; width=&quot;8.07&quot; height=&quot;1737.40&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;654.64&quot; y=&quot;320.28&quot; width=&quot;16.71&quot; height=&quot;2105.55&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;658.96&quot; y=&quot;324.59&quot; width=&quot;8.07&quot; height=&quot;2096.91&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;671.34&quot; y=&quot;337.35&quot; width=&quot;16.71&quot; height=&quot;2088.47&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;675.66&quot; y=&quot;341.67&quot; width=&quot;8.07&quot; height=&quot;2079.83&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;688.05&quot; y=&quot;525.71&quot; width=&quot;16.71&quot; height=&quot;1900.12&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;692.37&quot; y=&quot;530.03&quot; width=&quot;8.07&quot; height=&quot;1891.48&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;704.76&quot; y=&quot;628.42&quot; width=&quot;16.71&quot; height=&quot;1797.40&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;709.08&quot; y=&quot;632.74&quot; width=&quot;8.07&quot; height=&quot;1788.76&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;721.46&quot; y=&quot;816.78&quot; width=&quot;16.71&quot; height=&quot;1609.05&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;725.78&quot; y=&quot;821.10&quot; width=&quot;8.07&quot; height=&quot;1600.41&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;738.17&quot; y=&quot;782.50&quot; width=&quot;16.71&quot; height=&quot;1643.33&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;742.49&quot; y=&quot;786.82&quot; width=&quot;8.07&quot; height=&quot;1634.69&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;754.88&quot; y=&quot;1159.08&quot; width=&quot;16.71&quot; height=&quot;1266.74&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;759.19&quot; y=&quot;1163.40&quot; width=&quot;8.07&quot; height=&quot;1258.10&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;771.58&quot; y=&quot;987.93&quot; width=&quot;16.71&quot; height=&quot;1437.90&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;775.90&quot; y=&quot;992.25&quot; width=&quot;8.07&quot; height=&quot;1429.26&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;788.29&quot; y=&quot;1364.51&quot; width=&quot;16.71&quot; height=&quot;1061.31&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;792.61&quot; y=&quot;1368.83&quot; width=&quot;8.07&quot; height=&quot;1052.67&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;804.99&quot; y=&quot;1415.87&quot; width=&quot;16.71&quot; height=&quot;1009.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;809.31&quot; y=&quot;1420.19&quot; width=&quot;8.07&quot; height=&quot;1001.32&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;821.70&quot; y=&quot;1706.81&quot; width=&quot;16.71&quot; height=&quot;719.01&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;826.02&quot; y=&quot;1711.13&quot; width=&quot;8.07&quot; height=&quot;710.37&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;838.41&quot; y=&quot;1741.09&quot; width=&quot;16.71&quot; height=&quot;684.73&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;842.73&quot; y=&quot;1745.41&quot; width=&quot;8.07&quot; height=&quot;676.09&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;855.11&quot; y=&quot;1775.37&quot; width=&quot;16.71&quot; height=&quot;650.45&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;859.43&quot; y=&quot;1779.69&quot; width=&quot;8.07&quot; height=&quot;641.81&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;871.82&quot; y=&quot;1809.53&quot; width=&quot;16.71&quot; height=&quot;616.29&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;876.14&quot; y=&quot;1813.85&quot; width=&quot;8.07&quot; height=&quot;607.65&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;888.52&quot; y=&quot;1878.09&quot; width=&quot;16.71&quot; height=&quot;547.73&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;892.84&quot; y=&quot;1882.41&quot; width=&quot;8.07&quot; height=&quot;539.09&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;905.23&quot; y=&quot;1929.45&quot; width=&quot;16.71&quot; height=&quot;496.38&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;909.55&quot; y=&quot;1933.77&quot; width=&quot;8.07&quot; height=&quot;487.74&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;921.94&quot; y=&quot;2083.52&quot; width=&quot;16.71&quot; height=&quot;342.30&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;926.26&quot; y=&quot;2087.84&quot; width=&quot;8.07&quot; height=&quot;333.66&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;938.64&quot; y=&quot;1860.89&quot; width=&quot;16.71&quot; height=&quot;564.94&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;942.96&quot; y=&quot;1865.21&quot; width=&quot;8.07&quot; height=&quot;556.30&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;955.35&quot; y=&quot;2032.16&quot; width=&quot;16.71&quot; height=&quot;393.66&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;959.67&quot; y=&quot;2036.48&quot; width=&quot;8.07&quot; height=&quot;385.02&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;972.06&quot; y=&quot;2066.32&quot; width=&quot;16.71&quot; height=&quot;359.50&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;976.38&quot; y=&quot;2070.64&quot; width=&quot;8.07&quot; height=&quot;350.87&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;988.76&quot; y=&quot;2083.52&quot; width=&quot;16.71&quot; height=&quot;342.30&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;993.08&quot; y=&quot;2087.84&quot; width=&quot;8.07&quot; height=&quot;333.66&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1005.47&quot; y=&quot;2100.60&quot; width=&quot;16.71&quot; height=&quot;325.23&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1009.79&quot; y=&quot;2104.92&quot; width=&quot;8.07&quot; height=&quot;316.59&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1022.17&quot; y=&quot;2066.32&quot; width=&quot;16.71&quot; height=&quot;359.50&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1026.49&quot; y=&quot;2070.64&quot; width=&quot;8.07&quot; height=&quot;350.87&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1038.88&quot; y=&quot;2288.95&quot; width=&quot;16.71&quot; height=&quot;136.87&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1043.20&quot; y=&quot;2293.27&quot; width=&quot;8.07&quot; height=&quot;128.23&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1055.59&quot; y=&quot;2100.60&quot; width=&quot;16.71&quot; height=&quot;325.23&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1059.91&quot; y=&quot;2104.92&quot; width=&quot;8.07&quot; height=&quot;316.59&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1072.29&quot; y=&quot;2288.95&quot; width=&quot;16.71&quot; height=&quot;136.87&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1076.61&quot; y=&quot;2293.27&quot; width=&quot;8.07&quot; height=&quot;128.23&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1089.00&quot; y=&quot;2237.59&quot; width=&quot;16.71&quot; height=&quot;188.23&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1093.32&quot; y=&quot;2241.91&quot; width=&quot;8.07&quot; height=&quot;179.59&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1105.71&quot; y=&quot;2288.95&quot; width=&quot;16.71&quot; height=&quot;136.87&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1110.03&quot; y=&quot;2293.27&quot; width=&quot;8.07&quot; height=&quot;128.23&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1122.41&quot; y=&quot;2323.11&quot; width=&quot;16.71&quot; height=&quot;102.72&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1126.73&quot; y=&quot;2327.43&quot; width=&quot;8.07&quot; height=&quot;94.08&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1139.12&quot; y=&quot;2323.11&quot; width=&quot;16.71&quot; height=&quot;102.72&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1143.44&quot; y=&quot;2327.43&quot; width=&quot;8.07&quot; height=&quot;94.08&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1155.83&quot; y=&quot;2306.03&quot; width=&quot;16.71&quot; height=&quot;119.79&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1160.14&quot; y=&quot;2310.35&quot; width=&quot;8.07&quot; height=&quot;111.15&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1172.53&quot; y=&quot;2340.31&quot; width=&quot;16.71&quot; height=&quot;85.51&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1176.85&quot; y=&quot;2344.63&quot; width=&quot;8.07&quot; height=&quot;76.87&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1189.24&quot; y=&quot;2357.39&quot; width=&quot;16.71&quot; height=&quot;68.44&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1193.56&quot; y=&quot;2361.71&quot; width=&quot;8.07&quot; height=&quot;59.80&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1205.94&quot; y=&quot;2391.67&quot; width=&quot;16.71&quot; height=&quot;34.16&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1210.26&quot; y=&quot;2395.99&quot; width=&quot;8.07&quot; height=&quot;25.52&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1222.65&quot; y=&quot;2357.39&quot; width=&quot;16.71&quot; height=&quot;68.44&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1226.97&quot; y=&quot;2361.71&quot; width=&quot;8.07&quot; height=&quot;59.80&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1239.36&quot; y=&quot;2340.31&quot; width=&quot;16.71&quot; height=&quot;85.51&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1243.68&quot; y=&quot;2344.63&quot; width=&quot;8.07&quot; height=&quot;76.87&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1256.06&quot; y=&quot;2340.31&quot; width=&quot;16.71&quot; height=&quot;85.51&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1260.38&quot; y=&quot;2344.63&quot; width=&quot;8.07&quot; height=&quot;76.87&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1272.77&quot; y=&quot;2374.47&quot; width=&quot;16.71&quot; height=&quot;51.36&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1277.09&quot; y=&quot;2378.79&quot; width=&quot;8.07&quot; height=&quot;42.72&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1289.47&quot; y=&quot;2323.11&quot; width=&quot;16.71&quot; height=&quot;102.72&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1293.79&quot; y=&quot;2327.43&quot; width=&quot;8.07&quot; height=&quot;94.08&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1306.18&quot; y=&quot;2408.75&quot; width=&quot;16.83&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1310.50&quot; y=&quot;2413.07&quot; width=&quot;8.19&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1323.01&quot; y=&quot;2340.31&quot; width=&quot;16.71&quot; height=&quot;85.51&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1327.33&quot; y=&quot;2344.63&quot; width=&quot;8.07&quot; height=&quot;76.87&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1339.72&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1344.04&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1356.42&quot; y=&quot;2374.47&quot; width=&quot;16.71&quot; height=&quot;51.36&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1360.74&quot; y=&quot;2378.79&quot; width=&quot;8.07&quot; height=&quot;42.72&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1373.13&quot; y=&quot;2340.31&quot; width=&quot;16.71&quot; height=&quot;85.51&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1377.45&quot; y=&quot;2344.63&quot; width=&quot;8.07&quot; height=&quot;76.87&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1389.84&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1394.16&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1406.54&quot; y=&quot;2357.39&quot; width=&quot;16.71&quot; height=&quot;68.44&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1410.86&quot; y=&quot;2361.71&quot; width=&quot;8.07&quot; height=&quot;59.80&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1423.25&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1427.57&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1456.66&quot; y=&quot;2374.47&quot; width=&quot;16.71&quot; height=&quot;51.36&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1460.98&quot; y=&quot;2378.79&quot; width=&quot;8.07&quot; height=&quot;42.72&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1473.37&quot; y=&quot;2391.67&quot; width=&quot;16.71&quot; height=&quot;34.16&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1477.69&quot; y=&quot;2395.99&quot; width=&quot;8.07&quot; height=&quot;25.52&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1506.78&quot; y=&quot;2374.47&quot; width=&quot;16.71&quot; height=&quot;51.36&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1511.10&quot; y=&quot;2378.79&quot; width=&quot;8.07&quot; height=&quot;42.72&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1523.49&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1527.81&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1540.19&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1544.51&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1556.90&quot; y=&quot;2391.67&quot; width=&quot;16.71&quot; height=&quot;34.16&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1561.22&quot; y=&quot;2395.99&quot; width=&quot;8.07&quot; height=&quot;25.52&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1573.61&quot; y=&quot;2340.31&quot; width=&quot;16.71&quot; height=&quot;85.51&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1577.92&quot; y=&quot;2344.63&quot; width=&quot;8.07&quot; height=&quot;76.87&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1590.31&quot; y=&quot;2374.47&quot; width=&quot;16.71&quot; height=&quot;51.36&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1594.63&quot; y=&quot;2378.79&quot; width=&quot;8.07&quot; height=&quot;42.72&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1607.02&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1611.34&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1623.72&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1628.04&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1640.43&quot; y=&quot;2391.67&quot; width=&quot;16.71&quot; height=&quot;34.16&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1644.75&quot; y=&quot;2395.99&quot; width=&quot;8.07&quot; height=&quot;25.52&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1673.84&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1678.16&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1707.26&quot; y=&quot;2391.67&quot; width=&quot;16.71&quot; height=&quot;34.16&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1711.57&quot; y=&quot;2395.99&quot; width=&quot;8.07&quot; height=&quot;25.52&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1723.96&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1728.28&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1774.08&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1778.40&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1790.79&quot; y=&quot;2391.67&quot; width=&quot;16.71&quot; height=&quot;34.16&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1795.11&quot; y=&quot;2395.99&quot; width=&quot;8.07&quot; height=&quot;25.52&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1807.49&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1811.81&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1840.90&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1845.22&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1857.61&quot; y=&quot;2391.67&quot; width=&quot;16.71&quot; height=&quot;34.16&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1861.93&quot; y=&quot;2395.99&quot; width=&quot;8.07&quot; height=&quot;25.52&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2007.97&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2012.29&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2058.09&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2062.41&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2108.20&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2112.52&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2158.32&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2162.64&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2408.92&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2413.24&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2609.52&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2613.84&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3177.53&quot; y=&quot;2408.75&quot; width=&quot;16.71&quot; height=&quot;17.08&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;3181.85&quot; y=&quot;2413.07&quot; width=&quot;8.07&quot; height=&quot;8.44&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;390.80&quot; y2=&quot;100.86&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2425.82&quot; x2=&quot;350.83&quot; y2=&quot;2425.82&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;2425.82&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,2425.82)&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1671.91&quot; x2=&quot;350.83&quot; y2=&quot;1671.91&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1671.91&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1671.91)&quot; text-anchor=&quot;middle&quot;&gt;20&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;918.01&quot; x2=&quot;350.83&quot; y2=&quot;918.01&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;918.01&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,918.01)&quot; text-anchor=&quot;middle&quot;&gt;40&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;164.22&quot; x2=&quot;350.83&quot; y2=&quot;164.22&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;164.22&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,164.22)&quot; text-anchor=&quot;middle&quot;&gt;60&lt;/text&gt;
	&lt;text x=&quot;190.71&quot; y=&quot;1294.96&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 190.71,1294.96)&quot; text-anchor=&quot;middle&quot;&gt;Density&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;3859.02&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;454.16&quot; y1=&quot;2489.19&quot; x2=&quot;454.16&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;454.16&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;1289.47&quot; y1=&quot;2489.19&quot; x2=&quot;1289.47&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1289.47&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.05&lt;/text&gt;
	&lt;line x1=&quot;2124.91&quot; y1=&quot;2489.19&quot; x2=&quot;2124.91&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.1&lt;/text&gt;
	&lt;line x1=&quot;2960.35&quot; y1=&quot;2489.19&quot; x2=&quot;2960.35&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2960.35&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.15&lt;/text&gt;
	&lt;line x1=&quot;3795.66&quot; y1=&quot;2489.19&quot; x2=&quot;3795.66&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3795.66&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.2&lt;/text&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2729.16&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;mobility_rate&lt;/text&gt;
&lt;/svg&gt;
</body></html>"></iframe>



In this graph I specified a shorter bin width than the rule-of-thumb. There is no right bin width in practice, it really depends on what you want the reader to take away from the picture. 

Next, because we are from UCSD, let's add UCSD to the graph to see how we compare against the entire distribution. If you recall, UCSD's mobility rate was equal to 0.0483275.


```stata
graph twoway histogram mobility_rate, xline(0.0483275)
```


                <iframe frameborder="0" scrolling="no" height="436" width="600"                srcdoc="<html><body>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
&lt;!-- This is a Stata 16.1 generated SVG file (http://www.stata.com) --&gt;

&lt;svg version=&quot;1.1&quot; width=&quot;600px&quot; height=&quot;436px&quot; viewBox=&quot;0 0 3960 2880&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot; xmlns:xlink=&quot;http://www.w3.org/1999/xlink&quot;&gt;
	&lt;desc&gt;Stata Graph - Graph&lt;/desc&gt;
	&lt;rect x=&quot;0&quot; y=&quot;0&quot; width=&quot;3960&quot; height=&quot;2880&quot; style=&quot;fill:#EAF2F3;stroke:none&quot;/&gt;
	&lt;rect x=&quot;0.00&quot; y=&quot;0.00&quot; width=&quot;3959.88&quot; height=&quot;2880.00&quot; style=&quot;fill:#EAF2F3&quot;/&gt;
	&lt;rect x=&quot;2.88&quot; y=&quot;2.88&quot; width=&quot;3954.12&quot; height=&quot;2874.24&quot; style=&quot;fill:none;stroke:#EAF2F3;stroke-width:5.76&quot;/&gt;
	&lt;rect x=&quot;390.80&quot; y=&quot;100.86&quot; width=&quot;3468.22&quot; height=&quot;2388.33&quot; style=&quot;fill:#FFFFFF&quot;/&gt;
	&lt;rect x=&quot;393.68&quot; y=&quot;103.74&quot; width=&quot;3462.46&quot; height=&quot;2382.57&quot; style=&quot;fill:none;stroke:#FFFFFF;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2425.82&quot; x2=&quot;3859.02&quot; y2=&quot;2425.82&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1990.46&quot; x2=&quot;3859.02&quot; y2=&quot;1990.46&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1555.09&quot; x2=&quot;3859.02&quot; y2=&quot;1555.09&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1119.72&quot; x2=&quot;3859.02&quot; y2=&quot;1119.72&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;684.24&quot; x2=&quot;3859.02&quot; y2=&quot;684.24&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;248.87&quot; x2=&quot;3859.02&quot; y2=&quot;248.87&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1441.32&quot; y1=&quot;2489.19&quot; x2=&quot;1441.32&quot; y2=&quot;100.86&quot; style=&quot;stroke:#C10534;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;454.16&quot; y=&quot;2054.93&quot; width=&quot;101.23&quot; height=&quot;370.89&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;458.48&quot; y=&quot;2059.25&quot; width=&quot;92.59&quot; height=&quot;362.25&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;555.39&quot; y=&quot;794.38&quot; width=&quot;101.23&quot; height=&quot;1631.45&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;559.71&quot; y=&quot;798.70&quot; width=&quot;92.59&quot; height=&quot;1622.81&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;656.62&quot; y=&quot;164.22&quot; width=&quot;101.35&quot; height=&quot;2261.60&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;660.94&quot; y=&quot;168.54&quot; width=&quot;92.71&quot; height=&quot;2252.96&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;757.97&quot; y=&quot;622.85&quot; width=&quot;101.23&quot; height=&quot;1802.97&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;762.29&quot; y=&quot;627.17&quot; width=&quot;92.59&quot; height=&quot;1794.33&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;859.20&quot; y=&quot;1420.69&quot; width=&quot;101.23&quot; height=&quot;1005.13&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;863.52&quot; y=&quot;1425.01&quot; width=&quot;92.59&quot; height=&quot;996.49&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;960.42&quot; y=&quot;1831.56&quot; width=&quot;101.23&quot; height=&quot;594.27&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;964.74&quot; y=&quot;1835.88&quot; width=&quot;92.59&quot; height=&quot;585.63&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1061.65&quot; y=&quot;2011.00&quot; width=&quot;101.35&quot; height=&quot;414.82&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1065.97&quot; y=&quot;2015.32&quot; width=&quot;92.71&quot; height=&quot;406.18&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1163.00&quot; y=&quot;2202.45&quot; width=&quot;101.23&quot; height=&quot;223.38&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1167.32&quot; y=&quot;2206.77&quot; width=&quot;92.59&quot; height=&quot;214.74&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1264.23&quot; y=&quot;2302.19&quot; width=&quot;101.23&quot; height=&quot;123.63&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1268.55&quot; y=&quot;2306.51&quot; width=&quot;92.59&quot; height=&quot;114.99&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1365.46&quot; y=&quot;2350.09&quot; width=&quot;101.23&quot; height=&quot;75.74&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1369.78&quot; y=&quot;2354.41&quot; width=&quot;92.59&quot; height=&quot;67.10&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1466.68&quot; y=&quot;2361.97&quot; width=&quot;101.35&quot; height=&quot;63.86&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1471.00&quot; y=&quot;2366.29&quot; width=&quot;92.71&quot; height=&quot;55.22&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1568.04&quot; y=&quot;2377.93&quot; width=&quot;101.23&quot; height=&quot;47.89&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1572.36&quot; y=&quot;2382.25&quot; width=&quot;92.59&quot; height=&quot;39.25&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1669.26&quot; y=&quot;2393.89&quot; width=&quot;101.23&quot; height=&quot;31.93&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1673.58&quot; y=&quot;2398.21&quot; width=&quot;92.59&quot; height=&quot;23.29&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1770.49&quot; y=&quot;2373.97&quot; width=&quot;101.23&quot; height=&quot;51.85&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1774.81&quot; y=&quot;2378.29&quot; width=&quot;92.59&quot; height=&quot;43.21&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1871.72&quot; y=&quot;2409.86&quot; width=&quot;101.35&quot; height=&quot;15.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1876.04&quot; y=&quot;2414.18&quot; width=&quot;92.71&quot; height=&quot;7.32&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1973.07&quot; y=&quot;2413.82&quot; width=&quot;101.23&quot; height=&quot;12.00&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;1977.39&quot; y=&quot;2418.14&quot; width=&quot;92.59&quot; height=&quot;3.36&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2074.30&quot; y=&quot;2401.94&quot; width=&quot;101.23&quot; height=&quot;23.88&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2078.62&quot; y=&quot;2406.26&quot; width=&quot;92.59&quot; height=&quot;15.24&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2175.53&quot; y=&quot;2421.86&quot; width=&quot;101.35&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2179.84&quot; y=&quot;2421.50&quot; width=&quot;92.71&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2276.88&quot; y=&quot;2421.86&quot; width=&quot;101.23&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2281.20&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2378.10&quot; y=&quot;2421.86&quot; width=&quot;101.23&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2382.42&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2479.33&quot; y=&quot;2417.90&quot; width=&quot;101.23&quot; height=&quot;7.92&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2483.65&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;0.72&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2783.14&quot; y=&quot;2421.86&quot; width=&quot;101.23&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;2787.46&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3086.94&quot; y=&quot;2421.86&quot; width=&quot;101.23&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;3091.26&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3694.43&quot; y=&quot;2421.86&quot; width=&quot;101.23&quot; height=&quot;3.96&quot; style=&quot;fill:#CAC27E&quot;/&gt;
	&lt;rect x=&quot;3698.75&quot; y=&quot;2421.50&quot; width=&quot;92.59&quot; height=&quot;4.68&quot; style=&quot;fill:none;stroke:#D7D29E;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;390.80&quot; y2=&quot;100.86&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2425.82&quot; x2=&quot;350.83&quot; y2=&quot;2425.82&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;2425.82&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,2425.82)&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1990.46&quot; x2=&quot;350.83&quot; y2=&quot;1990.46&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1990.46&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1990.46)&quot; text-anchor=&quot;middle&quot;&gt;10&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1555.09&quot; x2=&quot;350.83&quot; y2=&quot;1555.09&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1555.09&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1555.09)&quot; text-anchor=&quot;middle&quot;&gt;20&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1119.72&quot; x2=&quot;350.83&quot; y2=&quot;1119.72&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1119.72&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1119.72)&quot; text-anchor=&quot;middle&quot;&gt;30&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;684.24&quot; x2=&quot;350.83&quot; y2=&quot;684.24&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;684.24&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,684.24)&quot; text-anchor=&quot;middle&quot;&gt;40&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;248.87&quot; x2=&quot;350.83&quot; y2=&quot;248.87&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;248.87&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,248.87)&quot; text-anchor=&quot;middle&quot;&gt;50&lt;/text&gt;
	&lt;text x=&quot;190.71&quot; y=&quot;1294.96&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 190.71,1294.96)&quot; text-anchor=&quot;middle&quot;&gt;Density&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2489.19&quot; x2=&quot;3859.02&quot; y2=&quot;2489.19&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;454.16&quot; y1=&quot;2489.19&quot; x2=&quot;454.16&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;454.16&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;1475.47&quot; y1=&quot;2489.19&quot; x2=&quot;1475.47&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1475.47&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.05&lt;/text&gt;
	&lt;line x1=&quot;2496.90&quot; y1=&quot;2489.19&quot; x2=&quot;2496.90&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2496.90&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.1&lt;/text&gt;
	&lt;line x1=&quot;3518.34&quot; y1=&quot;2489.19&quot; x2=&quot;3518.34&quot; y2=&quot;2529.16&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3518.34&quot; y=&quot;2619.14&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.15&lt;/text&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2729.16&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;mobility_rate&lt;/text&gt;
&lt;/svg&gt;
</body></html>"></iframe>



So we can now see that UCSD has quite a high mobility rate. There is a lot we can do to still improve this graph. Let's (1) add a descriptive title (2) add so labels and (3) make the graph more aesthetically pleasing. 


```stata
graph twoway histogram mr_kq5_pq1, color(blue%40) ///
    || pcarrowi 40 0.06 40 0.05, lc(black) mcolor(black) ///
    text(40 0.06 "UCSD"  , place(e) size(small) color(black) ) /// 
    xline(0.0483275, lc(black) lp(dash)) ///
    xlabel(0(0.02)0.2) ///
    title("Distribution of Mobility Rates across U.S. Colleges") ///
    xtitle("Mobility Rate") ///
    ytitle("Density") ///
    note("Source: Opportunity Insights") ///
    legend(off) ///
    graphregion(color(white) fcolor(white)) 


```


                <iframe frameborder="0" scrolling="no" height="436" width="600"                srcdoc="<html><body>&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot; standalone=&quot;no&quot;?&gt;
&lt;!-- This is a Stata 16.1 generated SVG file (http://www.stata.com) --&gt;

&lt;svg version=&quot;1.1&quot; width=&quot;600px&quot; height=&quot;436px&quot; viewBox=&quot;0 0 3960 2880&quot; xmlns=&quot;http://www.w3.org/2000/svg&quot; xmlns:xlink=&quot;http://www.w3.org/1999/xlink&quot;&gt;
	&lt;desc&gt;Stata Graph - Graph&lt;/desc&gt;
	&lt;rect x=&quot;0&quot; y=&quot;0&quot; width=&quot;3960&quot; height=&quot;2880&quot; style=&quot;fill:#EAF2F3;stroke:none&quot;/&gt;
	&lt;rect x=&quot;0.00&quot; y=&quot;0.00&quot; width=&quot;3959.88&quot; height=&quot;2880.00&quot; style=&quot;fill:#FFFFFF&quot;/&gt;
	&lt;rect x=&quot;2.88&quot; y=&quot;2.88&quot; width=&quot;3954.12&quot; height=&quot;2874.24&quot; style=&quot;fill:none;stroke:#FFFFFF;stroke-width:5.76&quot;/&gt;
	&lt;rect x=&quot;390.80&quot; y=&quot;275.35&quot; width=&quot;3468.22&quot; height=&quot;2099.36&quot; style=&quot;fill:#FFFFFF&quot;/&gt;
	&lt;rect x=&quot;393.68&quot; y=&quot;278.23&quot; width=&quot;3462.46&quot; height=&quot;2093.60&quot; style=&quot;fill:none;stroke:#FFFFFF;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2311.35&quot; x2=&quot;3859.02&quot; y2=&quot;2311.35&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1931.55&quot; x2=&quot;3859.02&quot; y2=&quot;1931.55&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1551.87&quot; x2=&quot;3859.02&quot; y2=&quot;1551.87&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1172.07&quot; x2=&quot;3859.02&quot; y2=&quot;1172.07&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;792.40&quot; x2=&quot;3859.02&quot; y2=&quot;792.40&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;412.60&quot; x2=&quot;3859.02&quot; y2=&quot;412.60&quot; style=&quot;stroke:#EAF2F3;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;2374.71&quot; x2=&quot;1261.63&quot; y2=&quot;2317.17&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;2288.33&quot; x2=&quot;1261.63&quot; y2=&quot;2230.66&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;2201.95&quot; x2=&quot;1261.63&quot; y2=&quot;2144.28&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;2115.57&quot; x2=&quot;1261.63&quot; y2=&quot;2057.90&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;2029.07&quot; x2=&quot;1261.63&quot; y2=&quot;1971.52&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;1942.69&quot; x2=&quot;1261.63&quot; y2=&quot;1885.14&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;1856.31&quot; x2=&quot;1261.63&quot; y2=&quot;1798.76&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;1769.93&quot; x2=&quot;1261.63&quot; y2=&quot;1712.38&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;1683.55&quot; x2=&quot;1261.63&quot; y2=&quot;1626.00&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;1597.17&quot; x2=&quot;1261.63&quot; y2=&quot;1539.62&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;1510.79&quot; x2=&quot;1261.63&quot; y2=&quot;1453.24&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;1424.41&quot; x2=&quot;1261.63&quot; y2=&quot;1366.86&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;1338.03&quot; x2=&quot;1261.63&quot; y2=&quot;1280.36&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;1251.65&quot; x2=&quot;1261.63&quot; y2=&quot;1193.98&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;1165.27&quot; x2=&quot;1261.63&quot; y2=&quot;1107.60&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;1078.76&quot; x2=&quot;1261.63&quot; y2=&quot;1021.22&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;992.38&quot; x2=&quot;1261.63&quot; y2=&quot;934.84&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;906.00&quot; x2=&quot;1261.63&quot; y2=&quot;848.46&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;819.62&quot; x2=&quot;1261.63&quot; y2=&quot;762.08&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;733.24&quot; x2=&quot;1261.63&quot; y2=&quot;675.70&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;646.86&quot; x2=&quot;1261.63&quot; y2=&quot;589.32&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;560.48&quot; x2=&quot;1261.63&quot; y2=&quot;502.94&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;474.10&quot; x2=&quot;1261.63&quot; y2=&quot;416.56&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;387.72&quot; x2=&quot;1261.63&quot; y2=&quot;330.05&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1261.63&quot; y1=&quot;301.34&quot; x2=&quot;1261.63&quot; y2=&quot;275.35&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;454.16&quot; y=&quot;1987.73&quot; width=&quot;82.79&quot; height=&quot;323.62&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;458.48&quot; y=&quot;1992.05&quot; width=&quot;74.15&quot; height=&quot;314.98&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;536.95&quot; y=&quot;888.43&quot; width=&quot;82.79&quot; height=&quot;1422.92&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;541.27&quot; y=&quot;892.75&quot; width=&quot;74.15&quot; height=&quot;1414.28&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;619.74&quot; y=&quot;338.71&quot; width=&quot;82.91&quot; height=&quot;1972.64&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;624.06&quot; y=&quot;343.03&quot; width=&quot;74.27&quot; height=&quot;1964.00&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;702.65&quot; y=&quot;738.81&quot; width=&quot;82.79&quot; height=&quot;1572.54&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;706.97&quot; y=&quot;743.13&quot; width=&quot;74.15&quot; height=&quot;1563.90&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;785.44&quot; y=&quot;1434.55&quot; width=&quot;82.79&quot; height=&quot;876.80&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;789.76&quot; y=&quot;1438.87&quot; width=&quot;74.15&quot; height=&quot;868.16&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;868.23&quot; y=&quot;1792.95&quot; width=&quot;82.79&quot; height=&quot;518.40&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;872.55&quot; y=&quot;1797.27&quot; width=&quot;74.15&quot; height=&quot;509.77&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;951.02&quot; y=&quot;1949.49&quot; width=&quot;82.91&quot; height=&quot;361.86&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;955.34&quot; y=&quot;1953.81&quot; width=&quot;74.27&quot; height=&quot;353.22&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1033.93&quot; y=&quot;2116.44&quot; width=&quot;82.79&quot; height=&quot;194.91&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1038.25&quot; y=&quot;2120.76&quot; width=&quot;74.15&quot; height=&quot;186.27&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1116.72&quot; y=&quot;2203.44&quot; width=&quot;82.79&quot; height=&quot;107.91&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1121.04&quot; y=&quot;2207.76&quot; width=&quot;74.15&quot; height=&quot;99.27&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1199.51&quot; y=&quot;2245.27&quot; width=&quot;82.79&quot; height=&quot;66.08&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1203.83&quot; y=&quot;2249.59&quot; width=&quot;74.15&quot; height=&quot;57.44&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1282.30&quot; y=&quot;2255.66&quot; width=&quot;82.91&quot; height=&quot;55.69&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1286.62&quot; y=&quot;2259.98&quot; width=&quot;74.27&quot; height=&quot;47.05&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1365.21&quot; y=&quot;2269.52&quot; width=&quot;82.79&quot; height=&quot;41.83&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1369.53&quot; y=&quot;2273.84&quot; width=&quot;74.15&quot; height=&quot;33.19&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1448.00&quot; y=&quot;2283.51&quot; width=&quot;82.79&quot; height=&quot;27.84&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1452.32&quot; y=&quot;2287.83&quot; width=&quot;74.15&quot; height=&quot;19.20&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1530.79&quot; y=&quot;2266.06&quot; width=&quot;82.79&quot; height=&quot;45.29&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1535.11&quot; y=&quot;2270.38&quot; width=&quot;74.15&quot; height=&quot;36.65&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1613.58&quot; y=&quot;2297.37&quot; width=&quot;82.91&quot; height=&quot;13.98&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1617.90&quot; y=&quot;2301.69&quot; width=&quot;74.27&quot; height=&quot;5.34&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1696.49&quot; y=&quot;2300.83&quot; width=&quot;82.79&quot; height=&quot;10.52&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1700.81&quot; y=&quot;2305.15&quot; width=&quot;74.15&quot; height=&quot;1.88&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1779.28&quot; y=&quot;2290.44&quot; width=&quot;82.79&quot; height=&quot;20.91&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1783.60&quot; y=&quot;2294.76&quot; width=&quot;74.15&quot; height=&quot;12.27&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1862.07&quot; y=&quot;2307.89&quot; width=&quot;82.79&quot; height=&quot;3.47&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1866.39&quot; y=&quot;2307.03&quot; width=&quot;74.15&quot; height=&quot;5.17&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;1944.86&quot; y=&quot;2307.89&quot; width=&quot;82.91&quot; height=&quot;3.47&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;1949.17&quot; y=&quot;2307.03&quot; width=&quot;74.27&quot; height=&quot;5.17&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2027.77&quot; y=&quot;2307.89&quot; width=&quot;82.79&quot; height=&quot;3.47&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;2032.09&quot; y=&quot;2307.03&quot; width=&quot;74.15&quot; height=&quot;5.17&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2110.56&quot; y=&quot;2304.30&quot; width=&quot;82.79&quot; height=&quot;7.05&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;2114.88&quot; y=&quot;2307.03&quot; width=&quot;74.15&quot; height=&quot;1.59&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2359.05&quot; y=&quot;2307.89&quot; width=&quot;82.79&quot; height=&quot;3.47&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;2363.37&quot; y=&quot;2307.03&quot; width=&quot;74.15&quot; height=&quot;5.17&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;2607.41&quot; y=&quot;2307.89&quot; width=&quot;82.91&quot; height=&quot;3.47&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;2611.73&quot; y=&quot;2307.03&quot; width=&quot;74.27&quot; height=&quot;5.17&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;rect x=&quot;3104.39&quot; y=&quot;2307.89&quot; width=&quot;82.79&quot; height=&quot;3.47&quot; style=&quot;fill:#0000FF;fill-opacity:0.40&quot;/&gt;
	&lt;rect x=&quot;3108.71&quot; y=&quot;2307.03&quot; width=&quot;74.15&quot; height=&quot;5.17&quot; style=&quot;fill:none;stroke:#0000FF;stroke-opacity:0.40;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1456.66&quot; y1=&quot;792.40&quot; x2=&quot;1289.47&quot; y2=&quot;792.40&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1289.47&quot; y1=&quot;792.40&quot; x2=&quot;1328.09&quot; y2=&quot;771.36&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;line x1=&quot;1289.47&quot; y1=&quot;792.40&quot; x2=&quot;1328.09&quot; y2=&quot;813.43&quot; stroke-linecap=&quot;round&quot; style=&quot;stroke:#000000;stroke-width:8.64&quot;/&gt;
	&lt;text x=&quot;1572.49&quot; y=&quot;820.41&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:79.94px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;UCSD&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2374.71&quot; x2=&quot;390.80&quot; y2=&quot;275.35&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2311.35&quot; x2=&quot;350.83&quot; y2=&quot;2311.35&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;2311.35&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,2311.35)&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1931.55&quot; x2=&quot;350.83&quot; y2=&quot;1931.55&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1931.55&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1931.55)&quot; text-anchor=&quot;middle&quot;&gt;10&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1551.87&quot; x2=&quot;350.83&quot; y2=&quot;1551.87&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1551.87&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1551.87)&quot; text-anchor=&quot;middle&quot;&gt;20&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;1172.07&quot; x2=&quot;350.83&quot; y2=&quot;1172.07&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;1172.07&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,1172.07)&quot; text-anchor=&quot;middle&quot;&gt;30&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;792.40&quot; x2=&quot;350.83&quot; y2=&quot;792.40&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;792.40&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,792.40)&quot; text-anchor=&quot;middle&quot;&gt;40&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;412.60&quot; x2=&quot;350.83&quot; y2=&quot;412.60&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;300.72&quot; y=&quot;412.60&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 300.72,412.60)&quot; text-anchor=&quot;middle&quot;&gt;50&lt;/text&gt;
	&lt;text x=&quot;190.71&quot; y=&quot;1325.03&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; transform=&quot;rotate(-90 190.71,1325.03)&quot; text-anchor=&quot;middle&quot;&gt;Density&lt;/text&gt;
	&lt;line x1=&quot;390.80&quot; y1=&quot;2374.71&quot; x2=&quot;3859.02&quot; y2=&quot;2374.71&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;line x1=&quot;454.16&quot; y1=&quot;2374.71&quot; x2=&quot;454.16&quot; y2=&quot;2414.69&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;454.16&quot; y=&quot;2504.54&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;0&lt;/text&gt;
	&lt;line x1=&quot;788.29&quot; y1=&quot;2374.71&quot; x2=&quot;788.29&quot; y2=&quot;2414.69&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;788.29&quot; y=&quot;2504.54&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.02&lt;/text&gt;
	&lt;line x1=&quot;1122.41&quot; y1=&quot;2374.71&quot; x2=&quot;1122.41&quot; y2=&quot;2414.69&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1122.41&quot; y=&quot;2504.54&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.04&lt;/text&gt;
	&lt;line x1=&quot;1456.66&quot; y1=&quot;2374.71&quot; x2=&quot;1456.66&quot; y2=&quot;2414.69&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1456.66&quot; y=&quot;2504.54&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.06&lt;/text&gt;
	&lt;line x1=&quot;1790.79&quot; y1=&quot;2374.71&quot; x2=&quot;1790.79&quot; y2=&quot;2414.69&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;1790.79&quot; y=&quot;2504.54&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.08&lt;/text&gt;
	&lt;line x1=&quot;2124.91&quot; y1=&quot;2374.71&quot; x2=&quot;2124.91&quot; y2=&quot;2414.69&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2504.54&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.1&lt;/text&gt;
	&lt;line x1=&quot;2459.04&quot; y1=&quot;2374.71&quot; x2=&quot;2459.04&quot; y2=&quot;2414.69&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2459.04&quot; y=&quot;2504.54&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.12&lt;/text&gt;
	&lt;line x1=&quot;2793.28&quot; y1=&quot;2374.71&quot; x2=&quot;2793.28&quot; y2=&quot;2414.69&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;2793.28&quot; y=&quot;2504.54&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.14&lt;/text&gt;
	&lt;line x1=&quot;3127.41&quot; y1=&quot;2374.71&quot; x2=&quot;3127.41&quot; y2=&quot;2414.69&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3127.41&quot; y=&quot;2504.54&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.16&lt;/text&gt;
	&lt;line x1=&quot;3461.53&quot; y1=&quot;2374.71&quot; x2=&quot;3461.53&quot; y2=&quot;2414.69&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3461.53&quot; y=&quot;2504.54&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.18&lt;/text&gt;
	&lt;line x1=&quot;3795.66&quot; y1=&quot;2374.71&quot; x2=&quot;3795.66&quot; y2=&quot;2414.69&quot; style=&quot;stroke:#000000;stroke-width:5.76&quot;/&gt;
	&lt;text x=&quot;3795.66&quot; y=&quot;2504.54&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;.2&lt;/text&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;2614.56&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:99.99px;fill:#000000&quot; text-anchor=&quot;middle&quot;&gt;Mobility Rate&lt;/text&gt;
	&lt;text x=&quot;408.00&quot; y=&quot;2737.89&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:79.94px;fill:#000000&quot;&gt;Source: Opportunity Insights&lt;/text&gt;
	&lt;text x=&quot;2124.91&quot; y=&quot;215.98&quot; style=&quot;font-family:&#x27;Helvetica&#x27;;font-size:139.96px;fill:#1E2D53&quot; text-anchor=&quot;middle&quot;&gt;Distribution of Mobility Rates across U.S. Colleges&lt;/text&gt;
&lt;/svg&gt;
</body></html>"></iframe>



Ok so that was a lot of changes. Let's go through it one at a time.
<ul>
<li>Line 1: "color(blue%40)" changed the color of the histogram. The "%40" will make the bars translucent. This isn't really so helpful here, but occasionally you will see pictures with two overlaid histograms. It is easier to see both if you make them translucent.

<li>Line 2: This adds an arrow to the graph that starts at (y1,x1) = (40, 0.06) and ends at the point (y2,x2) = (40, 0.05). The options "lc(black)" make the line part of the arrow black and the option "mcolor(black)" makes the head of the arrow black.

<li>Line 3: This adds the text "UCSD" to the point in the graph (y,x) = (40, 0.06). the option "place(e)" adjusts the location of the text just east of (40,0.06). The "size(small)" option changes the font to a smaller smize, and lastly "color(black)" makes the text of the word black.

<li>Line 4: This defines how the x-axis will appear. "0(0.02).02" tells STATA to make the x-axis go from 0 to 0.2 in increments of 0.02.

<li>Line 5: This gives the graph a descriptive title.

<li>Line 6 & 7: Titles the x and y-axis.

<li>Line 8: Adds a footnote to the graph.

<li>Line 9: Turns the legend off. Sometimes you will want a legend. For example, if there are multiple line plots or multiple histograms, you should give a clear label to each. In this case there is only a single histogram and I've already labelled it in the title.

<li>Line 10: This changes the background of the plot so that it is white instead of the light blue.
<ul>



```stata

```
