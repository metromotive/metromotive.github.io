---
layout: post
title: The Unreasonable Efficiency of Micromobility
category: blog
---

Over the last couple of decades, it's become economical and practical to augment bikes, scooters, skateboards, and a number of other types of small vehicles with battery-powered electric motors. 

[Horace Dediu](https://twitter.com/asymco) popularized the term "Micromobility" to refer to this class of vehicle: small (under 500 kg), usually electric, often shared, and used for utilitarian rather than recreational travel. 

I've had [some interest in this space for a while](https://metromotive.com/Hello-World/), and I thought I had a pretty good intuition around the energy required to move these vehicles (and their riders) around. 

But an [article by Matthew Yglesias](https://www.vox.com/the-goods/2018/9/10/17631318/electric-scooters-bird-city-regulations-sustainability) on the advantages of electric scooters—in this case over a combination of walking and transit—got me doing some math, and underscored just how impressive their efficiency is. 

My original conceit was to figure out how many avoided showers it would take before the direct energy use of the scooter was lower than a sweat-inducing alternative, but it depends on enough variables and assumptions that it's not an especially useful argument to get into[^1]. 

But what *is* instructive is to take an activity (a hot shower) that most Americans do on a near-daily basis and compare its energy use to what it takes to charge our micromobility benchmark, the humble [Segway ES1 electric kick scooter](http://www.segway.com/products/consumer-lifestyle/es1-kickscooter). 

Heating a liter of water from ground temperature (10°C) to a comfortable shower temperature (40°C) uses about 35 watt-hours of energy. A modern shower head has a flow rate of around 9 liters per minute, so a hot shower consumes about 315 Wh per minute[^2]. 

At an electricity cost of 10 cents per kilowatt hour (which is a typical retail rate in my corner of the United States), a ten-minute shower costs a little over 30¢, leaving aside the cost of acquiring and disposing of the water. 

A full charge on our scooter is something like 185 Wh, or roughly 200 Wh after accounting for the efficiency of the charger[^3]. Under typical use, that charge will be depleted in about 75 minutes. That leaves us with an energy consumption of about 2.7 Wh per minute. 

So a 10-minute scooter ride costs only a bit more than a *quarter of a penny*. Because it's difficult to reason about such a small number, it's more useful to look at what it would cost in electricity to make that trip every day for a year: The result is less than one dollar. 

Another interesting comparison is heating water for a hot beverage. A half-liter of water (Grandé in Starbucks parlance) heated from our local ground temperature to its boiling point uses around 50 watt hours, which is comparable to a 20-minute roundtrip on a scooter. 

Initially I wasn't sure if I was more impressed with the appalling wastefulness of heating water or the admirable thriftiness of moving a lightweight electric vehicle, but going a bit further I think there's a good argument for it being the latter. 

If we move from a small electric vehicle to a full-size car, we see that a [typical per-minute number](https://forums.tesla.com/forum/forums/what-your-whmile) is in the high two hundreds (assuming a freeway pace of a minute per mile)[^4].

It's fair to point out that the car in question will cover about five times more ground over the course of each minute, but looking at only aerodynamic drag, you would expect to beat the car's efficiency by around a factor of 25. Instead it's around a factor of one hundred[^5].

So it's clear that shifting traffic from private cars and ride hailing services to small electric vehicles could have a huge impact on transportation energy use, while having a negligible impact on household energy use. And it might even pay for itself in avoided showers. 

[^1]: Reasonable answers seemed to range from one avoided shower per week to one avoided shower every few years. 

[^2]: Let me acknowledge that watt hours per minute is kind of a weird combination of units, so if you prefer watts, just multiply everything by 60. 

[^3]: Of course a scooter in a dockless shared system might use quite a bit more energy after accounting for charging- and rebalancing-related transportation.

[^4]: Interestingly, a hot shower and a freeway drive in an efficient car use about the same amount of energy per minute. 

[^5]: This is mostly because it's *not* all about aerodynamic drag (drag area is actually pretty much the same in this comparison). There's some energy involved in climate-controlling the cabin, but it's mostly a relative of the "tyranny of the rocket equation", where the weight required to achieve and safely manage high speeds snowballs on itself.  
