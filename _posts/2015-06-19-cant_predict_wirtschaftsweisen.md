---
layout: post_page
title: Can't Predict? 
subtitle: The Wirtschaftsweisen
comments: true
---

![Black Swan](/img/black_swan.jpeg)

Tomorrow it'll be about 19°C and partly cloudy - at least that's what the
weather forecast tells me. I will be about 75 years old, when I die, and this
year we are supposed to have 2.77 million unemployed in Germany (about 130000
less then last year). Predictions about our future are everywhere. It is the
single reason for research in physics: To create models that predict how systems
will behave. We are so used to these forecasts and predictions that we hardly
ever question them. I started to be more doubtful about them when I first read
[The Black Swan](http://www.amazon.de/Black-Swan-Impact-Improbable-Incerto/dp/081297381X/ref=sr_1_1?ie=UTF8&qid=1434443002&sr=8-1&keywords=the+black+swan+nassim+taleb) by [Nassim
Taleb](http://en.wikipedia.org/wiki/Nassim_Taleb). He told me that humans are
quite bad at predicting anything that does not follow linear models and
normal distributions and that in fact virtually every important part of our life
falls in that category. But on the other hand you shouldn't believe what you
read and so I decided to find out myself if Nassim Taleb's raid on statistics is
reasonable or not. _Can't Predict?_ is supposed to be a series about
(scientific) predictions, which had some wider audience. I will try to test
whether they correct and to what extend. This first episode is about the
_Wirtschaftsweisen_...

Every year, shortly before Santa brings us presents, there is a group of old
men in Germany predicting the future. They are called the _Wirtschaftsweisen_,
the wise men of economics. When I was young and their future telling was
announced in television, I always thought of them as some kind of old, bearded
wise men of the orient, bringing gold and myrrh to little Jesus. As it turns out
they are not [particularly old, not at all bearded and not even all
men](http://www.sachverstaendigenrat-wirtschaft.de/ratsmitglieder.html) (though
80% are).  

Their official name is _Sachverständigenrat Wirtschaft_, and they (and their
scientific staff) predict a particular subset of the future: the economic
development of Germany. In 1964 this advisory board of economics (as the much
less bloomy official name translates) was establish to write a report every year
giving informed prospect to the economy of Germany and to some lesser extend
the rest of the world. The report is intended to help politicians make their
decisions based on scientific reasoning. Thus it is state funded and independent
(as far as this is possible).  Also, as state funded research, [their reports
are available
online](http://www.sachverstaendigenrat-wirtschaft.de/fruehere_jahresgutachten.html)
for free. It is very interesting to skim through those reports for various
reasons. First it is fun to see how the design evolves. From the nice and clear
60ies typefaces to the more states-men like serif fonts of the 80ies to
something which horribly resembles a late 90ies Word document in 2000. I wonder
if [Clippy](https://en.wikipedia.org/wiki/Office_Assistant) played a significant
role in the design process. They got nicer again nowadays.

But that's of course not the most interesting part of it. What was very
intriguing for me was the fact that in the beginning the focus is more on broad
developments, told by words. No single number was deemed so important but their
combination gave a direction. Of course some numbers where more important than
others. Especially the growth of the [gross national
product](https://en.wikipedia.org/wiki/Gross_national_product)
(_Bruttosozialprodukt_) was the number that predicted the wealth of the nation.
In the 1990ies that changed to the growth of the [gross domestic
product](https://en.wikipedia.org/wiki/Gross_domestic_product)
(_Bruttoinlandsprodukt_). Still you had to search for that number in huge
tables. In the late 90ies they started to print that number bold. And in recent
editions you find that number already in the preface. It seems like our whole
wealth is reduced to that one dimensional number.

As this number is now the superstar of economic key numbers I will test, how
well the wise men do in predicting that number. If you are interested in the
analysis I made, have a look at the [code](https://github.com/hildensia/hildensia.github.io/blob/master/data/cant_predict/Can't%20Predict%3F%20The%20Wirtschaftsweise.ipynb).

First we have to understand what data we have. The gross domestic product is the
sum of the value of all goods (read services plus things) produced within one
country within one year. Even measuring such a number is not an easy task, let
alone predicting it. The world bank as well as the statistical agency of Germany
provides us with annual measurements of the GDP since 1970. Let us believe in
these measurements for now. One particular difficulty is that we measure the
GDP in terms of money, but the value of money changes over time (loosely
speaking this is called inflation if it looses value and deflation if it gains).
So if we look at the absolute GDP numbers, they are not directly comparable
between years.  Thus both prediction as well as growth rates are provided in a
inflation-adjusted manner. To regain absolute values we have to use a curve
called GDP deflator.

If we compute the absolute values from the growth values we have to multiply by
the deflator to get meaningful numbers. The base year for that deflator is 2005,
which means that the nominal GDP (the number you read in the newspaper) and the
real value (the inflation adjusted) are the same for that year. For all other
years we would compute the values in prices of 2005. If we adjust by the GDP
deflator we gain back the prices of each year, which let us compare the
predictions to the actual values of each year.

For a sanity check I first adjust the measured growth rate provided by the world
bank with the GDP deflator and check if it matches the absolute GDP values
provided by the statistical agency.

 <iframe src="/data/cant_predict/sanity.html" scrolling="no" onload='javascript:resizeIframe(this);'></iframe>

Well, they match pretty much, up to some rounding errors introduced by the fact
that only two digit precision is provided by both the world bank as well as the
statistical agency. But our method seems to work. If we compute the mean error
it is about 1%, which is okay. Now we compare the predicted
growth with the actual growth.

 <iframe src="/data/cant_predict/prediction.html" scrolling="no" onload='javascript:resizeIframe(this);'></iframe>

On a first glance that looks also quite okay, but let's compare with the
simplest method, we can imagine: predict the growth of the GND of today for next
year. A prediction that is worth anything should at least beat that. Every child
could predict the current announced GND growth for the next year. We also test 
a bit more informed version: the mean growth so far as growth prediction, i.e.
we predict for 1986 the mean GND growth from 1970 to 1985.

 <iframe src="/data/cant_predict/all_abs.html" scrolling="no" onload='javascript:resizeIframe(this);'></iframe>

The easy guess looks even better, don't you think? Okay, we definitely need some
measures, to scientifically compare. But simply using the error or squared error
between the curves gives wrong impressions, because the Wirtschaftsweisen adjust
their prediction every year, whereas here I developed the curve from 2005 and
made no readjustment. That way the prediction seems less reliable than it really
os. So I use the actual GND of each year as base and compute
the absolute prediction. From that I measure the error. The error for all
prediction methods is

 * Easy guess error:  40.07 billion EUR
 * Mean growth error: 32.06 billion EUR
 * Prediction error:  38.50 billion EUR

Wait. The mean growth error is smallest? And all the errors are not
statistically significantly different! That means, if I simply assume the
current growth rate for the next year I'm within the range of the
Wirtschaftsweisen. If I assume the mean growth rate so far I'm probably better!
But that's not the whole story. Let's look further into the numbers. Let's look
at the predicted growth.

 <iframe src="/data/cant_predict/growth.html" scrolling="no" onload='javascript:resizeIframe(this);'></iframe>


This is somewhat hard to read, but let's try (also use the zoom and move
function to get a better overview). First, what we see is, that the mean growth
stays somewhat stable around 2%. But that's not at all what the real growth rate
looks like. That one is going up and down. So the mean growth rate might make a
smaller error but it is really bad at seeing trends.  The easy guess also goes
up and down, but always one year later. But the changes are not that long term.
It often misses important changes (e.g. see the crisis around 2008). The
prediction on the other hand often follows the trend, but misses the right
number. We can also get numbers from that. In statistics we use a measure called
[(Pearson)
correlation](https://en.wikipedia.org/wiki/Pearson_product-moment_correlation_coefficient)
to see how closely related events from two different random variables are. If
they are just a linear function of the other it is 1 or -1 (then the one
variable goes up, where the other goes down).  If they are not related
whatsoever, the correlation is 0. Lets see how these things are correlated to
the actual growth rate. The Pearson correlation is computed as (If you don't
understand that, simply ignore the math)


$$ \mathrm{Kor}_e (x,y) = \frac{ \sum_{i=1}^n (x_i-\bar x)(y_i-\bar y) }{ \sqrt{ \sum_{i=1}^n(x_i-\bar x)^2\cdot \sum_{i=1}^n(y_i-\bar y)^2 } } $$

where $\bar x$ and $\bar y$ are the mean values.

If we compute this value for all prediction variants we get:

* Correlation prediction/growth:  0.61
* Correlation easy guess/growth:  0.17
* Correlation mean growth/growth: 0.42

So what we learn from that is that the Wirtschaftsweisen aren't simply stupid.
They do a good job in analyzing the current situation, they are just not good at
predicting the absolute value of the growth. But that's no surprise. Many factors
play a role in the value, you see at the end of the year, lots' of them not even
objective and all highly coupled and non linear. But they seem to understand bigger
things. For example all recessions in Germany followed certain important events:

 * the [first oil crisis](https://en.wikipedia.org/wiki/1973_oil_crisis)
 * the [Iran-Iraq war](https://en.wikipedia.org/wiki/Iran%E2%80%93Iraq_War)/second oil crisis
 * the [gulf war](https://en.wikipedia.org/wiki/Gulf war) and the [fall of the wall](https://en.wikipedia.org/wiki/Die_Wende) and [the iron curtain](https://en.wikipedia.org/wiki/Revolutions_of_1989)
 * [9/11](https://en.wikipedia.org/wiki/September_11_attacks)
 * the [global financial crisis](https://en.wikipedia.org/wiki/Financial_crisis_of_2007%E2%80%9308)

(I hope I don't interpret too much into that.)

No easy guess or mean model would have shown these events. And though the
scientists highly underestimated the results of these events, their trend
prognosis was quite okay. So we should stop looking at the growth rate
prediction and start reading their whole report, which gives much more
information. And maybe they should stop giving this number as the main aspect of
their report. They even started to give a higher precision number recently.
While in older times they only had 0.5% steps, now they give a one-decimal
number (e.g. 1.6%). Without any scientific reason to belief that this precision
is of any worth. What would be interesting is is the uncertainty they have over
their predictions. I didn't find it somewhere in the report (but I might have
missed it).

Finally another good question is, if the GND is actually predictive for anything
important (e.g. how satisfied people are with their situation) or if it just
measures the income situation of a nation, no matter whether that is important.
Maybe the [Happy Planet Index](https://en.wikipedia.org/wiki/Happy Planet Index)
is much better...
But unfortunately this is out of scope of this blog post.

