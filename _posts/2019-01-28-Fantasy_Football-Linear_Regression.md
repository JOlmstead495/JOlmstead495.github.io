---
layout: post
title: Fantasy Football The Data Scientist Way
---

# Life, Death, Fantasy Football

Fantasy Football is one of those "hobbies" that I shouldn't put alot of time and effort into, and yet, I do. I'm not sure where the main part of my addiction comes from, yes there is money on the line, but its not much. I'm guessing the drive comes from wanting to outwit my friends; to prove that I am better than them in Fantasy Football (and also everything else).  
Naturally, now that I am in the genesis of my data science career, I want to use the data science toolset to embarrass my friends. With Linear Regression I did just that. Linear Regression is a great tool to use to analyze features' effect on a dependent value as well as predict values based on the features. I specifically used Linear Regression to predict individual players' fantasy football points for the next season. Below I'll explain how I did this, what tools I used, and some of the lessons I learned!

# Data Scraping

I scraped my data from [ESPN](http://www.espn.com/) and [Pro Football Reference](https://www.pro-football-reference.com/). In order to get Fantasy Points, I used [Selenium](https://www.seleniumhq.org/) to access ESPN. Selenium is a great library that allows users to, through python code, access websites and interact with them. ESPN only listed statistics on 50 players at a time so we need to use Selenium to interact with the website and iteratively go to the next 50 players. Selenium allows us to do this seamlessly. One quick lesson here, in order to have Selenium click on a specific element (in my case the next button), that element needs to be visible on the screen. You can use the below code to scroll down the website.  

``` (python)
driver.execute_script("window.scrollBy(0,200)","")
```

The actual statistics I scraped from ProFootball Reference using [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/). Beautiful Soup isn't nearly as fancy as Selenium, and basically just pulls the HTML code from the website. Pro Football Reference had all their data together on one page, so it was easier to pull with Beautiful Soup. If you are attempting to scrape data for a project, I would recommend using Beautiful Soup if you can, and then resorting to Selenium if you need a more interactive interface to the web page. That is just because Beautiful Soup is much easier to use.  
As far as the data cleaning goes, I removed any player who scored 0 Fantasy Points one year and had less than 100 yards rushing and receiving the year prior. I also split my data into two different datasets, one for Running Backs, and the other for Wide Receivers.  

# Model Testing

I started out by using Linear Regression to get a base model to compare future models to. I ran Linear Regression on all features of both RBs and WRs in order to get a baseline model that I can compare to other models down the road. I then ran Lasso Linear Regression as a feature selection for those models, which greatly cut my features down (from about 22 to 4). This also improved my model, so I continued to use the selected features from the Lasso model.  
After analyzine the distribution of the independent value for my tables observations, I realized that the target data was heavily right skewed. I attempted to transform the target data. I used logging, boxcox, and square root. None provided a substantial improvement to my model, so I decided to stick with a non transformed target. This decision will help my analaysis later on in the project. After going through this transformation and other changes to the model, I decided a normal Linear Regression model was the way to go.

# Model Results

My model was able to explain some variance. The WR model had an R Squared score of .58, while my RB model had an R Squared score of .45. The Root Mean Squared Error was around 50 fantasy points.  

# Model Prediction

Below are my top 5 predicted Fantasy Performers for next year. There are alot of improvements that can be made to this model, that I will be implementing over the next few months, so please do not blame me if you use these predictions and lose out bad next year..

## Top 5 Running Backs

<center>
| Player              | Predicted Fantasy Points |
|---------------------|-------------------------:|
|Christain McCaffrey  |                       322|
|Saquon Barkley       |                       310|
|Ezekiel Elliott      |                       273|
|Alvin Kamara         |                       270|
|James White          |                       260|
</center>

## Top 5 Wide Receivers

<center>
| Player              | Predicted Fantasy Points |
|---------------------|-------------------------:|
|Julio Jones          |                       322|
|Tyreek Hill          |                       310|
|DeAndre Hopkins      |                       273|
|Mike Evans           |                       270|
|JuJu Smitch-Schuster |                       260|
</center>