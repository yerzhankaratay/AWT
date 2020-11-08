---
title: "How many followers would Lin Dan have, had he an Instagram profile?"
date: 2019-04-23T10:07:47+06:00
draft: false

# post thumb
image: "../../images/post/201904-lin-dan-insta-badminton-followers/how-many- followers-would-lin-dan-have.png"

# meta description
description: "How many followers would Lin Dan have"

# taxonomies
categories:
  - "R"
tags:
  - "Badminton"
  - "Data Science"
  - "Sports"
  - "Regression Model"
  - "Statistics"
  - "Lin Dan"
  - "Son Wan Ho"
  - "Chen Long"
  - "Shi Yuqi"
  - "Instagram"

# post type
type: "post"
socialshare: true
---
What if...? Since Instagram and other social media are not accessible in China, its top athletes have to use local solutions. Lin Dan is one of the [China's top marketable athletes](https://www.scmp.com/sport/china/article/2047319/lin-dan-perhaps-chinas-most-marketable-athlete-so-will-his-many-sponsors) that has become a two-time Olympic, five-time World, as well as a six-time All England champion. Marketers could use his image cause badminton is the second most popular sport in the world, but it seems like it is not being used to its full potential, which is why I am curious.

## Data and Model’s Assumptions

I’ve made a linear regression model that tries to answer that question theoretically based on the number of followers other badminton players have considering their age, home country population, and career earnings from [Badminton World Federation](bwfbadminton.com)’s profiles, assuming that earnings correlate with fame the players get. For instance, badminton is big in Indonesia and young sportsmen are followed by a million people but the official profile of retired&nbsp;[Taufik Hidayat](https://www.instagram.com/taufikhidayatofficial/)&nbsp;only has under 190k followers currently, so one can assume that age correlates rather negatively, which I would say because older people seem to be reluctant to start and maintain a social media profile. I also assume that the number of followers positively correlates with a home country’s population, thus, the model assumes that Instagram is accessible in China.

I used the&nbsp;[list of countries by population (United Nations)](https://en.wikipedia.org/wiki/List_of_countries_by_population_(United_Nations))&nbsp;to merge it with previously gathered data of badminton players which includes country, career earnings, birth year and followers.

That is of course considering they had registered their instagram profiles at around the same time and kept them public, cause time is&nbsp;of importance to accumulate followers.

## Regression Analysis Results
```
linearmodel <- lm(Followers~Population2017+CareerEarnings+Age, data=model)

summary(linearmodel)
```
> Residual standard error: 360300 on 44 degrees of freedom
>
> Multiple R-squared: 0.2604,	Adjusted R-squared: 0.21
> F-statistic: 5.164 on 3 and 44 DF, &nbsp;p-value: 0.003813

p-value suggests that the model is statistically significant as it is smaller than any common level of significance, so I can assume that prediction results are explained by the model.

Now I have also prepared a sample with unknown to see how this linear regression model works:
![image](../../images/post/201904-lin-dan-insta-badminton-followers/00.png)

Running ```predict(linearmodel,prediction)``` results in:

Chen Long: 878529.1  
Shi Yuqi: 768120.9  
Lin Dan: 596571.1  
Taufik Hidayat: NA  
Son Wan Ho: 166731.7  
Lee Yong Dae: 300816.6

## Interpretation of Results

Son Wan Ho and Lee Yong Dae are from Korea which explains the reason why they would have less followers even when their career earnings are comparable to Chinese players, age here works in favor of Shi Yuqi and against Lin Dan, who would have 600k followers approximately and whose number is comparable to Lee Chong Wei’s 529k at the time of data collection, while Chen Long’s career earnings would make him the most followed player in the sample, as he’s a 30-year-old 1-time Olympic and 2-time world champion.&nbsp;

You may ask ‘why do Chinese players not have much more followers here?’, because Indian players are there in the dataset with comparable population but their players do not get as much attention as Indonesians get from their supporters.

Now, Taufik Hidayat's BWF Profile doesn't show his career earnings, and it is quite absurd to calculate money that he earned based on Indonesia’s population, his instagram followers and age but let’s do it for fun:
```
CareerEarn <- lm(CareerEarnings~Population2017+Followers+Age, data=model)
TaufikHCareerEarn <- predict(CareerEarn, prediction[4,])
TaufikHCareerEarn
```
```606796.1```
