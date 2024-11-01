---
title: "Blog Post 6"
author: "Kelly Olmos"
date: "2024-10-14"
categories: []
tags: []
slug: "blog-post-6"
---

In this week's blog post I will explore the role of campaign spending on advertisements in presidential elections. Does spending on ads impact electoral outcomes? I will also update my election forecast model. 

What does the literature say about ads and their effect on election outcomes? An experiment from Huber and Arceneaux (2007) concluded that advertising does not inform or mobilize voters. However, they do find evidence that ads persuade voters. How long do the persuasive effects of ads last? The persuasive effects of a televised ad typically last one to two weeks. Therefore, campaigns should be purchasing ads throughout the duration of the campaign to ensure that the persuasive effects of the ads are maximized (Gerber et al.). 

###The role of Ads in Previous Presidential Elections 


<img src="{{< blogdown/postref >}}index_files/figure-html/descriptive-statistics-3.png" width="672" />

In this graph above, four plots are displayed. It shows the top five issues in presidential elections from 2000 to 2012. One thing to note is that the scale on the x-axis is changing every year. It is increasing which indicates that there is an increase in ads aired. One of the prevalent issues in these graphs is economy-related. Taxes, economic policy, jobs, and employment are an issue voters care about. 


<img src="{{< blogdown/postref >}}index_files/figure-html/descriptive-statistics-6.png" width="672" />

Not only have the number of ads aired each election cycle been increasing but so has the amount that campaigns spend on ads. This makes sense, more ads aired, more money spent on ads. In the graph above you can see how the y-axis has different scales to reflect the increasing amount of money spent on ads. 2008 looks like the exception but it probably has to do with the fact that the graph plots until October 1, not November like the rest of the plots on the graph. The graph reflects that campaigns spend much more on ads as the election date approaches. This is consistent with the findings from (in-text citation) because campaigns want voters to have a favorable opinion about their candidate and maximize the persuasive effects of ads. 


<img src="{{< blogdown/postref >}}index_files/figure-html/descriptive-statistics-11.png" width="672" />


<img src="{{< blogdown/postref >}}index_files/figure-html/descriptive-statistics-12.png" width="672" />

Since social media has taken off, it has also been leveraged by campaigns. Campaigns can run ads on social media to reach more people. 

“So far in the 2024 election cycle, candidates, parties, and other groups have spent more than $619,090,533 on digital advertising concerning the election and political issues on the nation’s two largest online platforms, Google (which includes YouTube, Search, and third-party advertising) and Meta.” (Brennan Center for Justice) 

With that being said, Facebook ads for the Biden campaign in the 2020 election cycle. The two graphs above demonstrate the number of ads and the amount spent on ads on Facebook. There is a positive correlation between the number of ads and the amount spent on ads reaching almost $6 million dollars at its peak. 

Elections are becoming more expensive and advertisements are one of the contributing factors to the increasing cost of running a campaign. I think it’s important to consider how the cost may make running a campaign inaccessible for candidates and its time to think about introducing financial limits. Independent expenditures are also another factor to consider because they’re exempt from political financial limits. 

### Update to the Forecast 

I think economic indicators are strong predictors for which candidate will win this election. Although we are not in a recession and the economy seems to be doing fine, American voters do not feel that way because inflation has increased the cost of living and it doesn’t align with the economic growth of the country. For that reason I will be adding disposable income as a predictor for my regression model. Even as inflation has decreased in recent months, prices are still high and voters are reminded of it every time they fill up their gas tank or shop for groceries. (add in text citation) 

I obtained data on personal disposable income by state from the U.S. Bureau of Economic Analysis and it changed my forecast drastically. In last week’s blog post, all the swing states had gone for Harris. Now, Harris loses Arizona, North Carolina, and Georgia. The final forecast for this week reflects a win for Trump 281-257. 

I obtained confidence intervals by running 10000 simulations. 

|party      | total_electors|
|:----------|--------------:|
|Democrat   |            257|
|Republican |            281|

<img src="{{< blogdown/postref >}}index_files/figure-html/bayesianism-1.png" width="672" />

### References

GERBER, ALAN S., et al. “How large and long-lasting are the persuasive effects of televised campaign ads? results from a randomized field experiment.” American Political Science Review, vol. 105, no. 1, Feb. 2011, pp. 135–150, https://doi.org/10.1017/s000305541000047x. 

Huber, Gregory A., and Kevin Arceneaux. “Identifying the persuasive effects of presidential advertising.” American Journal of Political Science, vol. 51, no. 4, Oct. 2007, pp. 957–977, https://doi.org/10.1111/j.1540-5907.2007.00291.x. 

Petry, Eric, and Grady Yuthok Short. “Online Political Spending in 2024.” Brennan Center for Justice, 19 Sept. 2024, www.brennancenter.org/our-work/analysis-opinion/online-political-spending-2024. 

Rex, Kristina. “Voters Say Economy Is Number One Concern as People Struggle despite Shrinking Inflation.” CBS News, CBS Interactive, www.cbsnews.com/boston/news/voters-economy-2024-election/. Accessed 1 Nov. 2024. 