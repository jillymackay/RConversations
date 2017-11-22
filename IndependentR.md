---
title: "Losing the Training Wheels in R"
author: "Jill MacKay"
date: "22 November 2017"
output:
  html_document:
    keep_md: true
    theme: cosmo
    highlight: pygments
    toc: true 
    toc_float: true
    toc_depth: 4
    code_folding: hide
---
## About This Exercise
**Exercise Type:** Guidance

*Guidance documents explore how to practice R*

**Exercise Suitability:** Intermediate

*If you are getting comfortable running your own code, this exericse will begin to push you forward*


# Finding Your R Independence
When I began learning R, I felt as though there was a huge leap from learning about a specific package or technique, and then actually building my own code and troubleshooting my problems. In fact, I only began to feel comfortable with this after getting to know the R community and realising that their solutions were not unlike the ones I was embarrassed about using. 

In this tutorial I want to explicitly talk about how you can become and independent R coder. 



# Beginning an Analysis
For years I ran most of my statistical analyses in GenStat, and I was comfortable there. I knew where all the tests lived, and if I didn't know - I knew I could find it. Whenever I used R, the blank console stared at me, expecting that I would be able to spew out formulae as easily as I could breathe. 

And then I would close R in a panic, returning to the software I was familiar with. 

One of my key realisations was that GUI software like GenStat or MiniTab also expects me to understand statistics. When I want to analyse some data, I first think "what is an appropriate test for this dataset?" I very often read through a textbook, or some google search results, to make an informed decision. Then, when I visit statistical software, I use my knowledge of that test to navigate the software. 

My problem came from the fact that I learned about statistics through trial and error *in these GUI softwares*. When I began, I did not ask "What is an appropriate test for this dataset?", I asked "What can I do to this data?", and I only grasped the importance of approaching data with a test in mind later on. 

It's tempting to cluck your tongue and point out that I should have had a better understanding of statistics before I started to play with software - but it would be completely wrong. There is a great deal of evidence in educational research which suggests that most effective way to teach someone is to have them do the skill^[1](https://dl.acm.org/citation.cfm?id=950595),^ ^[2,](https://www.researchgate.net/profile/Richard_Felder/publication/279589632_Learning_by_doing/links/559aa7cb08ae793d13820e03.pdf)^ ^[3](http://www.sciencedirect.com/science/article/pii/S0140673600042215)^. And it is much, *much* easier to learn statistics in R than in a software like Genstat or Minitab. 

So the first key message I want you to walk away with is that you absolutely can make mistakes in R, and those mistakes can absolutely be fundmamental statistical mistakes. In many ways, R will help you make fewer mistakes, because it's cleverer than programs like GenStat and MiniTab. And if someone criticises you for making those mistakes, grit your teeth, and keep going. You will only learn from them. (And maybe point them in the direction of this document and the last section).



## Choosing a Test and a Package
One of the benefits of R being open source is the large R community. Let's say you were interested in the `ChickWeight` dataset (another dataset which comes from the package `datasets`). Your first step may even be to Google "[analysing ChickWeights dataset](https://www.google.co.uk/search?q=analysing+ChickWeights+dataset&rlz=1C1GGRV_enGB751GB751&oq=analysing+ChickWeights+dataset&aqs=chrome..69i57.6167j0j7&sourceid=chrome&ie=UTF-8)" which gives you a bunch of peoples' analyses (some good, some more intermediate) for that data. 

But it's more likely you'll be in a situation where you want to analyse a dataset that doesn't exist in R. Let's pretend therefore that `ChickWeight` is brand new for us. `ChickWeight` looks like this:


```r
#This table requires the knitr package
library(knitr)
kable(head(ChickWeight), format = "html", col.names = c("Chick Weight in Grams (weight)", "Chick Age in Days (Time)", "Chick ID (Chick)", "Chick Diet (Diet)"), caption="The effect of diet on chick growth (ChickWeight)")
```

<table>
<caption>The effect of diet on chick growth (ChickWeight)</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> Chick Weight in Grams (weight) </th>
   <th style="text-align:right;"> Chick Age in Days (Time) </th>
   <th style="text-align:left;"> Chick ID (Chick) </th>
   <th style="text-align:left;"> Chick Diet (Diet) </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 42 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 51 </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 59 </td>
   <td style="text-align:right;"> 4 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 64 </td>
   <td style="text-align:right;"> 6 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 76 </td>
   <td style="text-align:right;"> 8 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 1 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 93 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:left;"> 1 </td>
   <td style="text-align:left;"> 1 </td>
  </tr>
</tbody>
</table>

Let's say we decide we want some form of mixed model for this dataset, because we know we've recorded the same chicks multiple times. 

I would google either "[mixed models in r](https://www.google.co.uk/search?q=mixed+models+in+r&rlz=1C1GGRV_enGB751GB751&oq=mixed+models+in+r&aqs=chrome..69i57j0l5.2184j0j7&sourceid=chrome&ie=UTF-8)" or "[packages for mixed models in r](https://www.google.co.uk/search?rlz=1C1GGRV_enGB751GB751&ei=76UVWoe0J8HUwQK6iJT4Dw&q=packages+for+mixed+models+in+r&oq=packages+for+mixed+models+in+r&gs_l=psy-ab.3...31945.33520.0.33794.13.13.0.0.0.0.135.904.12j1.13.0....0...1.1.64.psy-ab..1.6.489...0i13k1j0i7i30k1j0i8i7i30k1j35i39k1.0.jQFg2ZFSWPo)". 

One of the first results comes from [R Bloggers](https://www.r-bloggers.com/linear-mixed-models-in-r/) who I quite like for quick overviews. I would immediately scroll down to the example in practice and start copying code myself. 


```r
#R Bloggers helpfully puts the required package at the top of their code! Good practice. 
library(nlme)
model.c <- lme (weight ~ Time*Diet, random = ~1|Time/Chick, data=ChickWeight)
summary(model.c)
```

```
## Linear mixed-effects model fit by REML
##  Data: ChickWeight 
##        AIC      BIC    logLik
##   5713.591 5761.393 -2845.795
## 
## Random effects:
##  Formula: ~1 | Time
##         (Intercept)
## StdDev:    4.878168
## 
##  Formula: ~1 | Chick %in% Time
##         (Intercept) Residual
## StdDev:    31.61389   11.871
## 
## Fixed effects: weight ~ Time * Diet 
##                  Value Std.Error  DF   t-value p-value
## (Intercept)  30.696773  4.985868 560  6.156756  0.0000
## Time          6.866491  0.396726  10 17.307907  0.0000
## Diet2        -2.063178  7.204117 560 -0.286389  0.7747
## Diet3       -12.446448  7.204117 560 -1.727685  0.0846
## Diet4         0.030768  7.222952 560  0.004260  0.9966
## Time:Diet2    1.742645  0.566736 560  3.074880  0.0022
## Time:Diet3    4.556380  0.566736 560  8.039685  0.0000
## Time:Diet4    2.858440  0.573093 560  4.987746  0.0000
##  Correlation: 
##            (Intr) Time   Diet2  Diet3  Diet4  Tm:Dt2 Tm:Dt3
## Time       -0.843                                          
## Diet2      -0.494  0.419                                   
## Diet3      -0.494  0.419  0.342                            
## Diet4      -0.492  0.418  0.341  0.341                     
## Time:Diet2  0.424 -0.508 -0.847 -0.293 -0.292              
## Time:Diet3  0.424 -0.508 -0.293 -0.847 -0.292  0.356       
## Time:Diet4  0.419 -0.502 -0.290 -0.290 -0.846  0.351  0.351
## 
## Standardized Within-Group Residuals:
##           Min            Q1           Med            Q3           Max 
## -1.4380602848 -0.1155068243  0.0009776385  0.0891521950  1.3261000592 
## 
## Number of Observations: 578
## Number of Groups: 
##            Time Chick %in% Time 
##              12             578
```
Here we can see that Diets 2, 3 and 4 are significantly different from Diet 1, and that [blah blah blah!]


# When Code Goes Bad

[Basics - google the error message, reproducible results...]



# A Word On Supporting People in R
I imagine Past-Jill coming to Present-Jill with an attempted analysis. The conversation might start like this:

> **Past-Jill:** I'm trying to analyse the infamous `ChickWeight` dataset that comes as standard in R. I want to run a regression between Diet and ChickWeight, but R keeps giving me an anova instead. I don't know what to do!

I hope that Present-Jill abides by the [Recuse Centre social rules](https://www.recurse.com/manual#sec-environment)...

* No feigning surprise
* No 'well actually's . . .
* No back-seat driving
* No subtle -isms


### No feigning surprise
> **Past-Jill:** I'm trying to analyse the infamous `ChickWeight` dataset that comes as standard in R. I want to run a regression between Diet and ChickWeight, but R keeps giving me an anova instead. I don't know what to do!

> **Present-Jill:** What? You don't know how to run a regression in R?!

No! This is not a compliment to Past-Jill. Why would Past-Jill be coming to me if she knew the answer? If someone comes to you, they're paying you the compliment of believing you'll know an answer. You don't need to show off.



### No 'well actually's . . . 
> **Past-Jill:** I'm trying to analyse the infamous `ChickWeight` dataset that comes as standard in R. I want to run a regression between Diet and ChickWeight, but R keeps giving me an anova instead. I don't know what to do!

> **Present-Jill:** Well, actually Diet is a categorical variable, obviously the regression won't work. 

The Recurse Centre say that 'well actually' is about the speaker 'grandstanding'. I think it also shows you haven't understood what somebody is asking. Try instead:



> **Present-Jill:** I can show you how to run a regression. What was your thinking behind using a regression for that particular dataset?  Are you familiar with categorical variables? I would consider diet a categorical variable, and so I'll also show you how to run an ANOVA and why that might be more appropriate here. 



### No back-seat driving
> **Past-Jill:** [*to fellow student*] I'm trying to analyse the infamous `ChickWeight` dataset that comes as standard in R. I want to run a regression between Diet and ChickWeight, but R keeps giving me an anova instead. I don't know what to do!

> **Present-Jill:** [*interjecting from across the room*] Try exploring what type of variable 'diet' is!

Intermittent interjections are not helpful. I'm particularly guilty of this I think. There's a bit of a double edged sword here too because often some people (e.g. women) may not be included in an R conversation. So instead of throwing out the odd thing, if you want to participate, do this:

> **Present-Jill:** [*standing up and walking over*] I'm interested in R, can I be part of this?

Either be in the conversation or be out - don't sit on the fence. 



### No subtle-isms
> **Past-Jill:** [*to fellow student*] I'm trying to analyse the infamous `ChickWeight` dataset that comes as standard in R. I want to run a regression between Diet and ChickWeight, but R keeps giving me an anova instead. I don't know what to do!

> **Present-Jill:** Sure, I'll help. You probably didn't get much of a chance to learn this at school in Glasgow, eh?

Subtle -isms make people feel unwelcome based on things we can't change. This is a really easy to mistake, and can be different for each person. The best way to deal with this is to acknowledge when it happens, apologise, and move on. 

> **Present-Jill:** Sure, I'll help. You probably didn't get much of a chance to learn this at school in Glasgow, eh?

> **Past-Jill:** Is that a wee Glasgow bias there? 

> **Present-Jill:** Sorry, was that a subtle-ism? I've been living in Edinburgh too long! Thanks for pointing that out. 

> **Past-Jill:** I think that might have been an Edinburgh bias on show there but let's stop this fictious conversation and get on with the R code. 



