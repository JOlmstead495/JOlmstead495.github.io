---
layout: post
title: Fantasy Football The Data Scientist Way
---

# Introduction

<p>Fantasy Football is one of those "hobbies" that I shouldn't put alot of time and effort into, and yet, I do. I'm not sure where the main part of my addiction comes from, yes there is money on the line, but its not much. I'm guessing the drive comes from wanting to outwit my friends; to prove that I am better than them in Fantasy Football (and also everything else).</p>
<p>Naturally, now that I am in the genesis of my data science career, I want to use the data science toolset to embarrass my friends. With Linear Regression I did just that. Linear Regression is a great tool to use to analyze features' effect on a dependent value as well as predict values based on the features. I specifically used Linear Regression to predict individual players' fantasy football points for the next season. Below I'll explain how I did this, and will point out pitfalls that I ran into while performing the analysis.</p>

# Data
<p> In order to get Fantasy Points total for a full season, I used [Selenium](https://www.seleniumhq.org/) to access ESPN. Selenium is a great library that allows users to, through python code, access websites and interact with them. The ESPN website had a table I could pull in, however it would only load 50 players at a time, so I needed to be able to click "next" on the website page to load the next 50 players. This is where I actually ran to my first pitfall of interest. You should expect Selenium to act just as you would when scrolling through a website. That means that if you are trying to click a specific item on the webpage, it actually needs to be visible in the actual browser. If 
the element is not visible, you can use the below code to scroll down the window until it is visible.</p>

``` (python)
driver.execute_script("window.scrollBy(0,200)","")
```

<p> The actual statistics I scraped from ProFootball Reference using [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/). Beautiful Soup isn't nearly as fancy as Selenium, and basically just pulls the HTML code from the website. Pro Football Reference had all their data together on one page, so it was easier to pull with Beautiful Soup. If you are attempting to scrape data for a project, I would recommend using Beautiful Soup if you can, and then resorting to Selenium if you need a more interactive interface to the web page. That is just because Beautiful Soup is much easier to use.</p>

<p> As far as the data cleaning goes, I removed any player who scored 0 Fantasy Points one year and had less than 100 yards rushing and receiving the year prior. I also split my data into two different datasets, one for Running Backs, and the other for Wide Receivers.</p>

# Model Testing
<p> I started out by using Linear Regression to get a base model to compare future models to. I ran Linear Regression on all features of both RBs and WRs. I then ran Lasso Linear Regression as a feature selection for those models, which greatly cut my features down (from about 22 to 4). This also improved my model, so I continued to use the selected features from the Lasso model.</p>

<p> After analyzine the distribution of the independent value for my tables observations, I realized that the data was heavily right skewed. I attempted to transform the target data. I used logging, boxcox, and square root. None provided a substantial improvement to my model, so I decided to stick with a non transformed target. This decision will help my analaysis later on in the project. After going through this transformation and other changes to the model, I decided a normal Linear Regression model was the way to go <p>


# Model Results
My model was able to explain some variance. The WR model had an R Squared score of .58, while my RB model had an R Squared score of .45. The Root Mean Squared Error was around 50 fantasy points.