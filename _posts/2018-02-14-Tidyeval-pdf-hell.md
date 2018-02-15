---
title: "Tidyeval meets PDF table hell"
layout: post
excerpt: Using tidy evaluation to clean up broken up values. 
category: rstats
tags:
  - tidyeval
  - tabulizer
  - tidyr
  - tidyverse
  - slice
image:
  feature: featureUnbreak.jpg
  credit: 
  creditlink: 
published: false
---
Tidyeval meets pdf table hell

Although it first became a feature of dplyr in June of 2017 
https://blog.rstudio.com/2017/06/13/dplyr-0-7-0/,  tidy evaluation is once again in the spotlight after the 2018 RStudio conference.  This is a good compilation of tidyeval resources, and I suggest watching this 
https://maraaverick.rbind.io/2017/08/tidyeval-resource-roundup/
this five-minute video of Hadley Wickham explaining the big ideas behind tidy evaluation while wearing a stylish sweater. 
https://www.youtube.com/watch?v=nERXS3ssntw
 
When tidyeval originally came out, I jumped at the chance to program with dplyr.  I blogged about writing a function to deal with non-data rows embedded as hierarchical headers in the data rectangle. Unsurprisingly, I butchered the use of tidyeval and function writing in general, but I was rescued by Jenny Bryan in this post.

As a biologist, the ‘untangle’ function has saved me hours upon hours of work, because comparative data always has taxonomic header rows that I usually had to tidy up by hand in a spreadsheet program. 

PDF table hell

In my ongoing work with other people’s data, I came across values that are broken up into two lines for whatever reason (often to optimize space on a page in a table in a typeset pdf).

I encounter broken-up values frequently in my biology research, here’s an example that isn’t made up.

<becerra>

This is a very common practice, a lot of the pdf tables that I work with (using the awesome tabulizer package) have ‘merged’ cells that end up as broken values.

Here’s a toy example with some data from the summer Olympics.

|Games            |Country   |Soccer_gold_medal |
|:----------------|:---------|:-----------------|
|Los Angeles 1984 |USA       |France            |
|Barcelona        |Spain     |Spain             |
|1992             |NA        |NA                |
|Atlanta 1996     |USA       |Nigeria           |
|Sydney 2000      |Australia |Cameroon          |
|London           |UK        |Mexico            |
|2012             |NA        |NA                |

The values for two of the games (Barcelona 1992 & London 2012) are broken up into separate rows, adding a bunch of empty/NA values in the rows that shouldn’t really be there. 

This is what the table should look like:

|Games_unbroken   |Country   |Soccer_gold_medal |
|:----------------|:---------|:-----------------|
|Los Angeles 1984 |USA       |France            |
|Barcelona 1992   |Spain     |Spain             |
|Atlanta 1996     |USA       |Nigeria           |
|Sydney 2000      |Australia |Cameroon          |
|London 2012      |UK        |Mexico            |


Using Jenny Bryan’s version of the untangle function as a template, I wrote this function to unbreak values using tidyeval. 

Assuming that:

> the NA values in the table only correspond to the rows with broken-up values,
> the broken-up values can be matched with regex 

this function will glue the two value fragments together and get rid of the extra row (via a hacky fill-then-slice operation). 

Let’s try it out.

After loading the tidyverse packages and rlang, we’ll create the above table, define the unbreak_vals function, and use it – matching the rows that start out with numbers with the regex.


{% highlight r %}

{% endhighlight %}


{% highlight r %}
{% endhighlight %}

{% highlight r %}
{% endhighlight %}

Another case of broken values that I’ve seen is when additional descriptions are interspersed below the original values in separate row. This is a single column example from a spreadsheet I had lying around.  

{% highlight r %}

{% endhighlight %}

{% highlight r %}

{% endhighlight %}

I have lots to learn about writing functions, but so far this unbreak function has already saved me lots of time and  hassle and painful spreadsheet editing.