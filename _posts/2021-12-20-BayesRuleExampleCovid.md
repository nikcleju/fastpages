---
toc: true
layout: post
description: Using the Bayes rule in a pandemic
categories: [md, bayes]
title: Bayes rule and vaccination benefit
published: true
---

# The Bayes rule

The Bayes rule is a fundamental formula in statistics which can be written as follows:
$$P(B | A) = \frac{P(A|B) \cdot P(B)}{P(A)}$$

We'll see below a nice example of using this in real life.

# Vaccination percentage in COVID-19 deceased

Suppose you browse Reddit and stumble upon the following data:

![]({{ site.baseurl }}/images/2021-12-20-CovidStats.png "Covid Stats")

What do the numbers tell us? The infections or death percentage seems much higher vor non-vaccinated people:

- Infections: 72.3% non-vaccinated vs 27.3% vaccinated
- Deaths: 92.1 % non-vaccinated vs just 7.9% vaccinated

Is it really the case?

## Intuition

These numbers don't tell the whole story. We may have an intuition about this if we ask the follwing question: is the ratio 72.3% vs 27.3%, the ratio of non-vaccinated to vaccinated people in new infections, telling us that vaccination makes a difference? Well, it depends on the vaccination percent of the **general population**:

- if the general population is also 72.3% not vaccinated and 27.7% vaccinated, then the share of vaccinated in new infections is the same as the share of vaccinated in the general population, which means than vaccination neither increases nor decreases the infection rate.
- if the vaccination rate in the general population is higher than the 27.7% among the infected, then we have a smaller slice of vaccinated people in new infections compared to the general population. In this case vaccination provides a benefit.
- if the vaccination rate in the general population is smaller than 27.7%, then we would have a larger slice of vaccinated in new infections compared to the general population. In this case vaccination would be detrimental.

(Note to self: To put a meme here! The one with Capaldi Dr Who.)

So, it seems that we must somehow consider the vaccination percentage in the whole population as well. Let's see how the Bayes rule comes to the rescue here.

## Problem definition

What is the proper name of the probability whose value is 72.3%? This is the share of non-vaccinated people within every 100 new infection cases. Therefore, it is a conditional probability. It's (mathematical) name is $P(\textrm{non-vaccinated | infected})$. In the fashion of Bayes rule, we write:
$$P(NV | I) = 72.3\%$$

Similarly, 27.3% is the conditional probability of being vaccinated if you are found to be infected:
$$P(V | I) = 27.7\%$$

Here, NV means "non-vaccinated", V means "vaccinated", and "I" means infected.

It is important to recognize these as **conditional probabilities**. They are conditional because they are computed for every 100 new infections, i.e. only out of these cases.

Now, the question we really want to know is: "how much does vaccination help in reducing the probability of infections?" That is, we would like to know the ratio of infected-if-vaccinated to infected-if-non-vaccinated:
$$\frac{P(I | V)}{P(I | NV)} = ?$$

Note the change in notation: we want `I|V` and `I|NV`, but what we have is actually the opposite, `NV|I` and `V|I`. This is exactly where the Bayes rule helps us.

## Applying the Bayes rule

Let's apply the Bayes rule to compute $P(I | V)$ and $P(I | NV)$ if we have the opposite values, $P(V | I)$ and $P(NV | I)$. We have:
$$P(I|V) = \frac{P(V|I) \cdot P(I)}{P(V)}$$
$$P(I|NV) = \frac{P(NV|I) \cdot P(I)}{P(NV)}$$

We want their ratio, and when dividing them the common term $P(I)$ simplifies away:
$$\frac{P(I | V)}{P(I | NV)} = \frac{P(V|I) \cdot P(I)}{P(V)} \cdot \frac{P(NV)}{P(NV|I) \cdot P(I)} = \frac{P(V|I)}{P(NV|I)} \cdot \frac{P(NV)}{P(V)}$$

The first term $\frac{P(V|I)}{P(NV|I)} = \frac{27.7\%}{72.3\%}$ comes from the initial data. In the second term, $P(V)$ and $P(NV)$ are the general vaccination and non-vaccination rate in the general population. The Bayes rule shows that we must consider this ratio as well in order to evaluate the actual benefit.

A quick search shows us that the vaccination rate in 17 october 2021 was around $P(V) = 29.4\%$, which means $P(NV) = 70.6\%$:

![]({{ site.baseurl }}/images/2021-12-20-VaccinationStats.png "Vaccination Stats")

We can now evaluate our result:
$$\frac{P(I | V)}{P(I | NV)} = \frac{P(V|I)}{P(NV|I)} \cdot \frac{P(NV)}{P(V)} = \frac{27.7\%}{72.3\%} \cdot \frac{70.6\%}{29.4\%} = 0.920$$

The numbers show that vaccination reduces the risk of infection by a merely 8%, which is not much. This is much different than what you might have expected from reading the initial percentages (Infections: 72.3% non-vaccinated, 27.7% vaccinated)

## How about the death rate?

Let's redo the computations for the death rate. We replace `I` with `D` in the formulas (D from "Deceased")

$$\frac{P(D | V)}{P(D | NV)} = \frac{P(V|D)}{P(NV|D)} \cdot \frac{P(NV)}{P(V)} = \frac{7.9\%}{92.1\%} \cdot \frac{70.6\%}{29.4\%} = 0.205$$

Vaccination reduces the risk of death by **five times** compared to non-vaccinated people. Great news and a major improvement, though not quite the ratio you might have expected from the original numbers (7.9% vs 92.1% means more than 10 times smaller, which is not the case).

## In retrospect

In retrospect, what the Bayes rule does here is not really magic. 
We can rewrite the desired ratio as follows:

$$\frac{P(D | V)}{P(D | NV)} = \frac{P(V|D)}{P(NV|D)} \cdot \frac{P(NV)}{P(V)} = \frac{\frac{P(V|D)}{P(NV|D)}} {\frac{P(V)}{P(NV)}}$$

What we want is actually the ratio of $\frac{P(V|D)}{P(NV|D)}$ to $\frac{P(V)}{P(NV)}$, i.e. the ratio of vaccinated-to-not-vaccinated among the deceased compared to the same vaccinated-to-not-vaccinated ratio in the general population. This makes sense, now that we see it, and we could have guessed it from the initial intution. However, one can easily be fooled by statistical data. It is great that the Bayes rule leads us nicely and rigorously to this result. 