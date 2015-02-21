---
layout:     post
title:      Shinyapps.io followup
date:       2015-02-21 14:20
summary:    My app's usage has reached a more reasonable level, and folks from RStudio got in touch to discuss active hours and paid plans.
comments:   True
published:  True
tags:       ['R', 'Shiny']
categories: ['R', 'Shiny']
---

In my last post, [Interactively visualizing my car's gas mileage with Shiny]({% post_url 2015-02-18-interactively-visualizing-my-cars-gas-mileage-with-shiny %}),
I griped about the number of active hours available for apps on
[shinyapps.io](http://www.shinyapps.io/)'s free plan. Free plans give you five
apps and 25 active hours per month. Overnight, my app had gone through
ten active hours and seemed on track to use up my entire allotment within
days. However, a few things have happened that make me think that post warrants
a followup.

### My app's usage has gone way down

The default amount of idle time before an app is stopped is 15 minutes, but you
can reduce it to as few as five minutes. After I did this, my app's usage
decreased dramatically. In this image the initial period of high usage all
occurs overnight (at least in the US), and the point where it drops
precipitously is right around when I changed the Instance Idle Timeout setting
the next day.

{% include figure.html src="/images/20150221_shiny_usage.png" caption="Graph showing usage of my Shiny app over time. Usage is very high for an initial period, then drops suddenly and stays fairly low." %}

Even after I blogged about my app and tweeted the blog post, usage
of my app stayed low. I may not use up all my hours with this one app after all.

I also realized that while I may see high-ish usage of this and future apps at
first, usage will almost certainly drop off after my tweets about my apps
(probably the main source of traffic) get buried under more recent tweets in my
feed. In later months I'll likely come in way under the hour limit. Which brings
me to:

### Conversations with RStudio

After my last post, I was contacted by Bill Carney and Tareef Kawaf of RStudio
to talk more about active hours and paid plans. A few helpful points came out of
our congenial and illuminating email exchange, including:

* 98% of shinyapps.io users use fewer than 10 hours per month on their
  apps. This makes 25 hours seem like a reasonable choice for free plans.
* The 25 hour limit for free accounts is not yet in effect (which makes sense
since paid accounts aren't yet available), so my app is safe for now.

We discussed what sort of monthly fee I would be willing to pay for a paid plan
and what that plan might look like. If 25 active hours/month indeed turns out to
be not enough, I personally would want a very cheap paid plan---definitely
<$10/month---that would give me more hours and the ability to deploy a few more
apps. Finances are part of the reason I'm not going to pay for the $39/month
plan, but the other reason is that most of my apps are likely to be small
personal projects that just aren't worth paying hundreds of dollars a year
for. I speculated that many hobbyists like myself are likely to be grad students
and wondered if educational discounts like the ones GitHub offers might be a
possibility.

It sounds like a sub-$10 plan isn't out of the question, but that RStudio needs
more data on who would use it and what features they want. Would you be
interested in such a plan? Feel free to weigh in in the comments.
