+++
title = "ARCCSS/CLEX Winter School 2018: Event Detection and Attribution"
date = 2018-07-13T19:20:25+03:00
draft = true

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = []
categories = []

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
# Use `caption` to display an image caption.
#   Markdown linking is allowed, e.g. `caption = "[Image credit](http://example.org)"`.
# Set `preview` to `false` to disable the thumbnail in listings.
[header]
image = ""
caption = ""
preview = true

+++

I attended the ARCCSS/CLEX winter school from 25th-29th June 2018. The theme of the winter school was climate extremes and so it involved lectures in the morning on various climate extremes (such as heatwaves and rainfall extremes) and also a group activity in the afternoons. The groups were tasked with producing analysis investigating one of five complex questions which are listed below.

- Project A: Examine the influence of modes of climate variability on temperature extremes over Australia
- Project B: Examine the influence of modes of climate variability on precipitation extremes over Australia
- Project C: Examine the role of soil moisture preconditioning on the frequency and/or intensity of heatwaves
- Project D: Examine changes in drought and what this means for impacts in Australia
- Project E: Pick a recent extreme event. What are the natural and anthropogenic influences?

Each group was comprised of 6-7 students with various backgrounds and at various stages of their education. I think Melissa Hart, our ARCCSS/CLEX graduate director, did a great job allocating students to various projects that highlighted their skills as well as gave them an opportunity to improve in their weak areas. Since my PhD has focussed heavily on observations, including the creation of a global dataset of daily precipitation, I have always felt like I could improve my climate model output analysis skills. As such I was very excited when I discovered that my group was allocated the fifth project, the detection and attribution project.

Right from the beginning I new what I wanted to get out of this group activity. I wanted to make sure that by the end of the group project I new how to design an experiment that involves analysing climate model simulations. This meant knowing what the different model output variables are, what the various runs are (historical, future RCP projections, various forcings, various initial conditions etc.), what the various climate models are in ensembles like the CMIP5 ensemble, the structure and location of these variables on Raijin ([Australia's supercomputer](nci.org.au)), and where to find information on these variables.

As a group we were keen to do event attribution on a recent event or at least an event that other reseearchers had not looked at yet. As a result we decided to look at the anomalously warm April we had this year. Based on the heatmap of temperature percentiles for 2018 on [isithotrightnow.com](https://isithotrightnow.com), we were aware of the heatwaves in Sydney, Adelaide, Canberra and ALice springs in the middle of April. Upon checking the Bureau of Metoerology website, we discovered that April 2018 saw record hot Tmax temperatures and second highest Tavg temperatures on record. Upon further research we discovered [a conversation article]() where Sarah Perkins-Kirpatrick was quoted saying that she would "put her house on the line that this had an anthropogenic influence". 

The experiment we decided to conduct was simple. The aim was to investigate whether the anomolously warm April temperatures was more likely in the current world compared to the natural world. We decided to look at the models in the CMIP5 ensemble to do this. There were 14 models with both a historical run with all forcings, including anthropogenic (historical-ALL), and a historical run with only natural forcings, no anthropogenic (historical-NAT). The plan was to look at however many years of simulations in the various runs of each of these models and make a distribution of April temperatures in historical-ALL forcings and compare it with the distribution in historical-NAT forcings. We would then compute the fraction of attributable risk (FAR) value using the probabilities of April 2018 temperatures greater than of equal to our chosen abosolute April threshold in these two scenarios (natural vs all-forcing). 

Since we are using an absolute threshold, however, we need to bias correct the models first. We do this by calculating the difference in means of April temperatures for Australia over 1961-1990 between historical-ALL runs of each model and observations, for which we used the AWAP dataset, and then adding this difference back to the April 2018 temperatures for this model. Upon a quick chat with [Andrew King](twitter.com/andrewking), I realised that bias correcting the models in this way assumes that the models are biased only in the means but not in the variance or skewness of the distribution. During our bias correction process, however, we realised that two of the models a;ldkfja; and adklfja; were still biased. This could have been because of an error in the calculation of the bias or perhaps the bias in these models is not systematic in time, meaning the bias over 1961-1990 is not the same as the bias over the entire period of run for this model. In the end we decided to not use these models. The biases in our models are shown below, with the two models which we did not use crossed out.

Our results (as seen below) show that the historical-ALL distribution is shifted to the right compared to the historical-NAT distribution, however, the variance of the historical-ALL distribution is also smaller compared to the variance of the historical-NAT distribution. As a result, total area underneath the pdf to the right of the threshold (shown by the vertical black line) is actually smaller in the historical-ALL distribution compared to the historical-NAT distribution. As a result, we attribute a FAR value of -10%! During my chat with Andrew King earlier, he had informed me that I can calculate the significance of my FAR value using bootstrapping. This is done by randomly sampling from both historical-ALL and historical-NAT runs and calculating the FAR value based on these. This process is repeated to create a distribution of our null hypothesis that the April 2018 temperatures are not more likely in the anthropogenic world. We can then test out FAR value against this distribution at a 10% level (5th and 95th percentile) check for significance. We did not have time to conduct this bootstrapping excercise during the winter school but I suspect, had we done that, we would have found that our FAR value is not significant. My understanding is that the April 2018 event we chose is too extreme to accurately estimate the tails of the distributions because of the lack of estimates around the threshold. I believe if we chose a less extreme event we could have gotten a significant FAR value. The other possibility is that models cannot actually produce temperature this extreme. Had we bias corrected the variance of this distribution, we could have estimated the tails of the distribution better. 

Figures.

I think when it comes to event attribution, the actual FAR values should be given less emphasis anyway because of the uncertainties associated with the models and observations. I think we should be satisfied with statements that just say if the event in quesiton is more or less likely in the anthropogenic world. Unfortunately we cannot with certainty make any such statements based on our analysis, although it looks like the event may not have an anthropogenic influence which does not bode well for Sarah's house! :tongue:

In terms of the future work related to this project, I clearly need to do the bias correction properly and also include significance based on the boostrapping excercise. Based on the BOM data we also discovered that these anomolous April temperature were also the most widespread across Australia. So another interesting atttribution excersie would be to attribute the spatial extent of this event to anthropogenic forcing. This can be done by once again chosing a local percentile based threshold for each grid cell to determine if the temperatures there are anomolous compared to the past. We can then count the number of grids experiencing anomolous temperatures during April 2018 in the historical-ALL vs the historical-NAT simulations. 

In summary, I thought the entire excercise served its main purpose which is that now I feel a lot more confident in designing and conducting experiments that involve CMIP5 model output analysis. A huge thanks goes out to Melissa Hart, out ARCCSS/CLEX graduate director, for a great winter school!
