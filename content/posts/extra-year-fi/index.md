+++
date = '2025-08-26T08:51:05-06:00'
draft = false
title = 'What does an "extra buffer" in FIRE mean?'
tags = ["finance"]
+++

{{< figure src=overview.png >}}

### Goal

The FIRE number can tell you how much you need to safely retire. A lot of people tend to go over that amount to feel 'really safe'. while this intuitively makes sense, it's a little less obvious what it does and how it makes the difference. This article proposes an intuition how to think about that 'extra' and how would it most likely influence your financial outcome.

*Disclaimer: This is not a financial advice, only an educational content. Speak to a financial (fixed fee ideally) advisor before making any financial decisions.*

<!-- ### SWR - Safe Withdrawal Rate

Safe Withdrawal Rate - is a % of your invested portfolio you can safely withdraw to sustain your financial goal. The common goals include:
  * I am 65, retiring now, I have investable pot that I want to last my retirement (often 30 years). A commonly used SWR now is 4%, however, the author of the 4% rule, Bill Bengen now proposes [5% is the actual safe number](https://www.amazon.com/Richer-Retirement-Supercharging-Spend-Enjoy/dp/1394343175).
  * I am 40, retiring **early**. I need money to last 50-60 years. A well known blog, [Early Retire Now](https://earlyretirementnow.com/safe-withdrawal-rate-series/), has produced a very extensive series, arriving at roughly 3.25% the safe number. This is the safe number to last indifinetely.

This is a big topic in itself, because the safe percentage is also influenced by how you withdraw money. The points above assume withdrawing the same amount each year, updated each year by inflation. 

There are other methods of withdrawing money, that allow to increase the SWR %.

For simplicity, let's assume the SWR is 4%. The model works also for any other SWR.
-->
### The FIRE number

The FIRE number - i.e. how much I need to retire now, is the amount of money I should invest to be able to sustain withdrawals for a given time period, or indefinitely.

For our example:
  * I need to spend 50,000 $ a year.
  * I am retiring at 65, need the income for 30 years.

Using 4% rule for simplicity, I can get the FIRE number.

```
50,000$ / 4% = 50,000 / 0.04 = 1,250,000$
```

**Note**: for withdrawals that last ~forever, use 3.25% (see [Early Retire Now](https://earlyretirementnow.com/safe-withdrawal-rate-series/)) and check out the tool below.

### When to stop, 'yet another year' syndrome

Let's say you amassed this amazing 1.25m$ in investable assets. But decided to work more and accumulate a bit more. What would that extra saved money do for your financial outcome?

Let's say, you saved a full 2m\\$. What does it mean?

### Bucketing


Let's start by splitting your money into two imaginary piles:
  * FIRE pile, 1.25m$ that you will use for withdrawals using 4% rule.
  * "The rest" pile, 0.75m$ that you will treat as pure growing investment.

{{< figure src=two-piles.png >}}

Imagine the FIRE pile is reserved for your withdrawals, and the "rest" pile grows as if you were still accumulating money. That "rest" pile is simply untouched, the FIRE pile pays for your expenses. We'll assume the FIRE pile maintains real value to isolate the effect of the buffer. In reality, it may rise of fall.

### Financial outcomes

Given that, we can model two piles differently:
  * FIRE pile will just maintain its value accounting for inflation (this is a conservative assumption, in reality it will more often go up, but can also go down especially if SWR is high).
  * The "rest" pile will grow using the rule of 72. If you expect real market return 7% (10% before inflation), the "rest" pile will double on average in 10 years. (10 * 7 ~ 72) 

In our example we start with 2m:
  * At 65: we have 1.25m FIRE + 0.75m growing pile = \$2m.
  * At 75: we have 1.25m FIRE + 1.5m (doubled) growing pile = \$2.75m
  * At 85: we have 1.25m FIRE + 3m (quadrupled) growing pile = \$4.25m

{{< figure src=two-piles-growth.gif >}}

If we zoom out to our whole 2m pile: we allow our full invested amount to grow, albeit more slowly than if we were growing the whole 2m. The whole amount (2m$) would double effectively in real money in just under 20 years.

### See it for yourself

Here is a simple tool where you can play with the SWR (safe withdraw rate) and the size of extra pile, to see how it influences your total amount over time. Could that convince you that you have enough **now** ?

{{< two_piles_tool >}}


A few notes:
  * If you enter 3.25% or lower SWR, the FIRE pile income should last over 50 years (see amazing series about it at [Early Retire Now](https://earlyretirementnow.com/safe-withdrawal-rate-series/)). This means you shouldn't worry about FIRE pile shrinking to zero.
  * If you enter 4% for SWR, this should last 30 years at 95% success rate. But at year 30, your FIRE pile can shrink to zero. However, "the rest" pile will effectively be 8x of its initial value which could compensate for that.
  * The tool assumes perfect conditions, the consistent returns each year. This is not true in reality. However, you should expect a similar trend in the value of your invested assets over time.
  * Real-world results will vary with market paths, taxes, fees, and rebalancing.
