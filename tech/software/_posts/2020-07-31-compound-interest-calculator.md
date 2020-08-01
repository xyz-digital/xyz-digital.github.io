---
layout: post
title:  Compound Interest Calculator
author: Adam Berg
---

> Compound interest is the 8th wonder of the world.  He who understands it, earns it; he who doesn’t, pays it.
> -- Albert Einstein

A simple tool to help visualize compound interest, followed by some general personal finance thoughts.

<!--more-->

## Calculator

Source for the tool can be explored [here](https://jsfiddle.net/devtails/71cjrfzg/2/)

<script src="https://code.highcharts.com/highcharts.src.js"></script>
<script src="https://momentjs.com/downloads/moment.min.js"></script>

<label for="start">Start</label>
<input id="start" name="start" type="date"/>

<label for="end">Date</label>
<input id="end" name="end" type="date"/>

<label for="principal">Principal</label>
<input id="principal" name="principal" type="number" value="0"/>

<label for="monthly">Monthly Deposit</label>
<input id="monthly" name="monthly" type="number" value="1000"/>

<label for="interest">Annual Interest Rate (%)</label>
<input id="interest" name="interest" type="number" value="6"/>

<label for="view">View</label>
<select id="view" name="view">
    <option value="month">Month</option>
    <option value="year" selected>Year</option>
</select>

<button id="submit">Submit</button>

<div id="container" style="width:100%; height:800px;"></div>
<script>
    function calulate() {
        const startInput = document.getElementById("start");
        const endInput = document.getElementById("end");
        const principalInput = document.getElementById("principal");
        const interestInput = document.getElementById("interest");
        const monthlyInput = document.getElementById("monthly");
        const viewInput = document.getElementById("view");

        let isMonthly = viewInput.value === "month";

        let startMoment = new moment(startInput.value);
        let endMoment = new moment(endInput.value);

        let principal = Number(principalInput.value);
        let monthlyDeposit = Number(monthlyInput.value);
        let periodMonths = endMoment.diff(startMoment, "month") + 1;
        let annualInterestRate = Number(interestInput.value) / 100;

        let compoundedData = [principal];
        let interestData = [0];
        let principalData = [principal];

        for (let i = 1; i < periodMonths; i++) {
            let lastAmount = compoundedData[i - 1];
            let newTotalBeforeInterest = lastAmount + monthlyDeposit;
            let newTotalAfterInterest = Math.round(newTotalBeforeInterest * (1 + annualInterestRate / 12));
            let interest = newTotalAfterInterest - newTotalBeforeInterest;

            principalData.push(principalData[i - 1] + monthlyDeposit);
            interestData.push(interestData[i - 1] + interest);
            compoundedData.push(newTotalAfterInterest);
        }

        let finalCompoundedData = [];
        let finalInterestData = [];
        let finalPrincipalData = [];

        let numPeriodsPerPoint = isMonthly ? 1 : 12

        for (let i = 0; i < periodMonths; i += numPeriodsPerPoint) {
            finalCompoundedData.push({
                y: compoundedData[i]
            });
            finalInterestData.push({
                y: interestData[i]
            });
            finalPrincipalData.push({
                y: principalData[i]
            });
        }

        var myChart = Highcharts.chart('container', {
            chart: {
                type: 'column'
            },
            title: {
                text: 'Compound Interest'
            },
            xAxis: {
                title: {
                    text: 'Month'
                },
                labels: {
                    formatter() {
                        let unit = isMonthly ? "month" : "y";
                        return moment(startMoment).add(this.value, unit).format("YYYY-MM");
                    }
                }
            },
            yAxis: {
                title: {
                    text: '$$$'
                }
            },
            series: [{
                name: "Compounded",
                data: finalCompoundedData
            }, {
                name: "Principal",
                data: finalPrincipalData
            },
            {
                name: "Interest",
                data: finalInterestData,
            }
            ]
        });
    }
    document.getElementById("submit").addEventListener("click", calulate);
    document.addEventListener('DOMContentLoaded', function () {
        document.getElementById("start").valueAsDate = new Date();
        document.getElementById("end").valueAsDate = moment().add(30, "y").toDate();
        calulate();
    });
</script>

## Who Wants to be a Millionaire?

![Who Wants to Be a Millionaire](https://media.giphy.com/media/3h4EfNUQR02HBk6hpU/giphy.gif)

I would be hard pressed to find someone that would answer no to that question.  What blows my mind is the number of people that choose to avoid thinking about money.  There is lots that can be said here, but for now I'll boil it down to the essentials.

### Budgeting

I stressed a lot about budgeting when I graduated from university.  [The Wealthy Barber](https://www.amazon.ca/Wealthy-Barber-Successful-Financial-Planning/dp/0773762167) by David Chilton is a straightfoward and fun introduction to the basics of personal finance.  His first and most important rule is "pay yourself first."  This is budgeting at it's simplest.  You know how much you make, and you know how much you spend.  Pick some amount that you can afford to put away every month and place it in safe interest bearing instruments.  The calculator above is designed to help visualize what different amounts will lead you towards (and when!).

### Investments

I'll defer most details to Chilton's book, but the simplest advice here is index funds.  If some people win people and some people lose hard, how does the average person fare?  "The average annualized total return for the S&P 500 index over the past 90 years is 9.8 percent".  <sup><a href="https://www.cnbc.com/2017/06/18/the-sp-500-has-already-met-its-average-return-for-a-full-year.html#:~:text=The%20average%20annualized%20total%20return,90%20years%20is%209.8%20percent.">1</a></sup>  Before trying that number out, take a guess at what the total value will be when going from 6% to 9.8%.  You will likely be surprised.

Chilton's point, and most other index fund proponents, is that with consistent and regular investment you guarantee that you will achieve this average performance.  When you're investing for the long term and regularly, in a lot of ways you are hoping for a downturn.  You have lost immediate liquidity of certain assets, but now everything is effectively on sale.  Trust that things will return (as they historically always have) and you will be rewarded with an even higher interest rate.  In the last 10 years (post 2008 financial crisis), the average compound annual return of the SPDR S&P 500 (SPY) ETF was 13.84%. <sup><a href="http://www.lazyportfolioetf.com/etf/spdr-sp-500-spy/#:~:text=In%20the%20last%2010%20years,with%20a%2013.33%25%20standard%20deviation.">2</a></sup>

### Don't Think About It

Wait, didn't I just say to think about it?  Yes, but just enough to convince yourself to start putting money away monthly.  That money is not to be touched essentially until retirement.  Every additional year you essentially receive a raise as your already compounded money starts making more.  

### What About __________________?

Life always finds a way to surprise you.  Keep an emergency fund so you don't need to withdraw investements when the next market downturn inevitably arrives or there is some kind of emergency.  Stopping monthly deposits for a year isn't going to set you back a huge amount.  Get yourself back on track and return when life permits.

### Conclusion

![Slow and Steady Wins the Race](https://media.giphy.com/media/6KoZJly4uLqMg/giphy.gif)

With an arguably low 6% return rate, I have shown that in 30 years you could have $1,000,000 for just $1,000 per month.  You put in $360,000 and get back $1,000,000.  If you can't afford that much, that's ok too.  The amount you need to save up for retirement is relative to your lifestyle.  There's lots more that can be said here, but I'll save it for another time.