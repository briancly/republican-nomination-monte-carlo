This is an adapted version of Gary Boone's Presidential Monte Carlo redesigned to fit the 2016 primaries, originally assuming a two-man race of Trump and Cruz. After working more with this model, I found that it's probably not the best with anything greater than a one-on-one competition, since it focuses on calculating the probability of gaining greater than 50% for a given state. In fact, the primary and caucus is much more complicated than that, due to states having different rules. Nonetheless, I've since updated it to factor in the remaining candidates, Rubio and Kasich, and set the threshold as Trump's supposed ceiling: 35%.

The original README text can be found below.

**DISCLAIMER**: I will concede that the data for polling this cycle seems to be extremely noisy, in Trump's favor when in actuality he doesn't really have that much dominance (or does he?) in the race. There's also the fact that I don't know what I'm doing. Or maybe we should dispel once and for all the fiction that I don't know what I'm doing. I know exactly what I'm doing.

To run:

	go run election2016.go state.go api.go cdf.go parse.go college.go

## Original README ##

election2012.go is a Monte Carlo simulator of the 2012 presidential election written in Go. 

* It reads polling data from the Huffington Post API.
* It ignores Rasmussen polls (http://fivethirtyeight.blogs.nytimes.com/2010/11/04/rasmussen-polls-were-biased-and-inaccurate-quinnipiac-surveyusa-performed-strongly/)
* It transforms the polling data into probabilities.
* It assumes that where there's no polling data, the state will vote as it did in 2008.
* It runs 25,000 simulations to determine the most likely electoral college total for Obama.

## Usage ##

To run:

	$ go run election2012.go state.go api.go cdf.go parse.go college.go

or

	$ go build election2012.go state.go api.go cdf.go parse.go college.go
	$ ./election2012

Try:

	$ ./election2012 -minStdDev 0.05
  
    $ ./election2012 -minStdDev 0.07 -acceptableSize 4000 -sims 50000
    Election 2012 Monte Carlo Simulation

    Collecting survey data for the great state of WA, WY, WV, PA, UT, NV, NY, NC, NE, ND, NH, NJ, NM, SD, SC, LA, OR, OK, OH, HI, MT, MS, MD, ME, MA, MN, MO, MI, FL, KS, KY, DC, DE, IL, IN, IA, ID, GA, CT, CO, CA, VA, VT, TN, AK, AL, TX, AR, AZ, RI, WI.

    Obama win probability: 84.59%. Average votes: 305.80

For additional details about the data that was gathered and the simulation, see the the logfile.

	$ less logfile


## Notes ##

Why does the Monte Carlo simulation make the election appear more certain for Obama than the national polls that show a close race?

First, you need to understand that the national polling reported in the media is irrelevant. 47% Obama, 46% Romney? Irrelevant. Why? Because popular polls don't elect the president; the electoral college does. To predict the winner, you need to use the polls to simulate the various combinations of electoral college wins/losses for each candidate. This simulation uses 25,000 simulated elections. Most of the time, Obama wins.

Second, you need to understand that if the polls are 52% Obama in a state with a margin of error of 4%, then Obama is 69% likely to win that state. You might think that 52% Obama in the polling means that he's 52% percent likely to win the state. But that's not how the statistics work. If you have a coin that comes out heads 52% of the time with a 4% margin of error, then that means that the probability of exceeding 50% is about 70%. To see this, go to a [cumulative distribution function calculator](http://www.danielsoper.com/statcalc3/calc.aspx?id=53) and plug in 52, 5, 50 and then calculate 1 - the answer given. In Obama's case, exceeding 50% is a win for that state. It turns out to have a probability of ~70%. 

Finally, realize that there is more certainty available in the data than is described in the polls. That's because 1) the electoral college elects presidents and 2) there are only so many ways that the electoral college can add up to a win for either candidate. For example, CA, HI, and NY will almost certainly vote for Obama; no need to poll in those states. That's why we talk about swing states; they can change the outcome. But there are only a limited number of ways the swing states can combine to a victory for one candidate or the other. As it turns out, the current polling makes these combinations favor Obama. It's nearly impossible for swing states to combine in ways that add up to a Romney win.


## Poll Data Source ##

The state-by-state presidential polling data is provided by the [Pollster API](http://elections.huffingtonpost.com/pollster/api).



## Sources of Error ##

* Polling data is combined by pooling multiple samples into a single large sample.
* Undecides and Others are ignored. For Others, that's not a bad assumption, as the contest remains a two-person contest among the two leaders. For Undecideds, it implies that they'll allocate themselves according to the current proportions for each candidate. But historically, late deciders favor challengers.
* Only state-by-state polling data is considered. Professional modelers, such as the Nate Silver at the excellent [FiveThirtyEight blog](http://fivethirtyeight.blogs.nytimes.com/) consider many other factors, adjustments, and trendlines as well. See [FiveThirtyEight's methodology](http://fivethirtyeight.blogs.nytimes.com/methodology/). 
* On FiveThirtyEight, click on the tab titled "Presidential Now-cast" to see Nate's results if the elction were held right now. Those results correspond to the results generated by this simulation. 


## License ##
The code is available at github [GaryBoone/PresidentialMonteCarlo](https://github.com/GaryBoone/PresidentialMonteCarlo) under [MIT license](http://opensource.org/licenses/mit-license.php).
