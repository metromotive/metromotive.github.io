---
layout: post
title: Inertial Electric Assist
category: blog
usemathjax: true
---

## Motivation

I'm currently working on an ebike build, and [the bike that I'm using](https://www.bikepedia.com/QuickBike/BikeSpecs.aspx?item=16723) has some quirks that make it difficult to fit a conventional bottom bracket torque sensor, or to add switches to the brake levers for activating regenerative braking (and such switches also only provide a single level of regenerative braking, rather than one that is proportional to the desired braking level). 

Having done a lot of [thinking about acceleration in micromobility vehicles](https://twitter.com/metromotive/status/1368745493912641540) recently, I came upon the idea of not actually measuring torque at all, but inferring it from other measurements. This was validated when a friend of mine mentioned a project that showed promising results from just such a scheme. 

What follows is a combination of planning and specualtion for this build, but is also might generalizable to other vehicle types. If it works, it would provide a potentially cheaper alternative to conventional torque/force sensors while also providing proportional regenerative braking. 

## Background

An electric assist vehicle is one where an electric motor multiplies the effort of a human to propel the vehicle. That is to say it incorporates a controller that seeks to make $$F_{motor} = F_{human} \cdot gain$$. This type of control scheme is extremely common for electric bicycles, as it provides a very intuitive way of controlling the motor's output. 

One of the difficulties in building an electric assist vehicle is measuring the human input in a way that is simple, reliable, and inexpensive. In the example of electric-assist bicycles, sensors typically measure pedal torque or chain tension, either directly or indirectly[^1]. These sensors can be complicated—often requiring a rotating electrical signal to be connected to a non-rotating control system—and require careful calibration. 

An alternative is to instead measure the acceleration of the vehicle using an inexpensive inertial measurement unit (IMU) circuit, and then measure, estimate, or be told the (gross) mass of the vehicle. Via Newton’s second law, we can arrive at the sum total of all of the forces acting on the vehicle. We then have to subtract all of the non-human forces applied to the vehicle to arrive at the human contribution to the propulsion force. 

The forces acting on a typical vehicle will be force from the propulsion and braking system ($$F_p$$), resistance from friction ($$F_r$$, proportional to the weight on the wheels), and resistance from whatever fluid the vehicle is moving through ($$F_d$$, proportional to the square of velocity). The force of gravity also plays a role, but because an IMU measures gravity as part of the overall acceleration, we can ignore it for purposes of this discussion. 

For an electric assist vehicle, we also want to decompose the propulsion force into the component provided by the motor ($$F_{motor}$$) and the part provided by the human ($$F_{human}$$), or requested by the human via their control over the friction brakes. So the overall equation is:

$$am = F_{motor} + F_{human} + F_r + F_d$$

Combining this with the basic equation of an electric assist vehicle, the desired force of the motor for the next time step can be calculated:

$$F_{motor}' = gain \cdot (am - (F_r + F_d + F_{motor}))$$

## Variables to be Measured or Estimated

### Acceleration

The acceleration can be measured using an inexpensive IMU (inertial measurement unit) integrated circuit. These are available at single-digit-dollar prices and use a MEMS (micro electro-mechanical system) mechanism to measure the acceleration, typically on three orthogonal axes. More expensive units use gyroscopic devices to measure rotation rates, but that information may be unnecessary for our application. 

The acceleration can also be measured by comparing successive measurements of speed from the vehicle's motor controller. This provides a slightly different picture of acceleration, as gravitational acceleration doesn't directly influence the measurement, which for this application is actually a bug rather than a feature. 

### Mass

Measuring the mass of the vehicle is more complicated. On the one hand it is unlikely to change significantly over the course of a trip, and it may be acceptable to require the rider to manually input their mass (plus that of any cargo).

Another option is to instruct the user to briefly not apply any human input, and have the motor apply a known propulsion force while measuring the resulting acceleration. 

Or the vehicle can be outfitted with strategically-placed load cells to measure the weight of the rider (along with that of some or all of the vehicle). 

### Motor Force

Modern motor controllers as a matter of course measure (and sometimes control) the amount of current flowing to the motor. Using the motor current, the torque constant of the motor, and the radius of the wheel (along with any gear reduction), the force provided by the motor can be calculated very accurately. 

### Rolling Resistance and Friction

The rolling resistance and friction present in a wheeled vehicle is fairly predictable, usually containing a constant component (present at all but zero speed) along with a component that increases more or less linearly with speed. Speed is another variable that is measured by modern motor controllers as part of their normal operation. 

### Aerodynamic Drag

The aerodynamic drag of a vehicle is harder to predict, as it depends on the drag coefficient (which could vary based on the rider, their clothing, and their position), the local density of air, the wind speed and direction, and the square of vehicle speed relative to the wind. 

Again, speed is straightforward to measure, and the apparent wind in the direction of travel can be measured by a relatively inexpensive anemometer, such as a Pitot tube connected to a differential pressure sensor. The local air density could potentially be measured as well (using a separate pressure absolute pressure sensor connected to the static port), but in practice it likely wouldn’t vary enough to matter. 

The drag coefficient would likely have to be estimated at a fixed value, although it is possible that the vehicle could detect time periods when there is no human force input and calculate an estimate of the aerodynamic drag based on knowledge of the other forces acting on the vehicle. 

### Gravity

On level ground, gravity can be estimated to have zero effect, since it will be exactly cancelled out by the normal force from the ground.

However the force of gravity becomes significant when on an incline. Fortunately an IMU will capture gravitational acceleration as part of its overall acceleration measurement, so no additional calculation is required. 

### Human Inputs

What’s left are human-initiated force inputs. On an electric-assist vehicle, this will be something like pedaling or pushing along with braking using friction brakes. 

Again, a number of systems exist to more directly measure human inputs, but it is also potentially feasible to calculate an estimate of human inputs by measuring acceleration and estimating and/or measuring all of the other forces acting on a vehicle along with its mass. 

## The Control Loop

The controller for such as system should have inputs consisting of, at a minimum:

- A measurement of acceleration
- An estimate or measurement of mass
- The current passing through the motor
- A desired gain factor for the amount of motor force to be applied for a given human force application

This is sufficient to determine (at low speeds and on level ground) whether the motor current should be increased or decreased to keep the motor force equal to the human-input force multiplied by the desired gain. 

The control loop can be enhanced with:

- An estimate of rolling friction (based on the motor speed)
- An estimate of aerodynamic drag (based on the apparent wind speed, or the square of the motor speed assuming still air)

On major enhancement would be the ability to detecting when the human force inputs are likely zero. For an electric-assist bicycle, the variation in torque due to the instantaneous pedal position could be detected when present, and thus when absent. Braking forces may be harder to detect since they don’t vary on a regular periodic basis, and thus it could be difficult to discriminate between an unknown braking force and an error in estimating one or more other variables. On the other hand, brake controls can be fitted with switches to detect when they are not being applied. 

When the human force inputs are determined to be zero, the control system could detect which parameters are varying based on the speed versus the square of speed versus the (odometric and/or inertial) acceleration. This should allow estimates of friction, drag, and mass to be refined over time. 

## Vehicle Applications

The vehicle types this could be applied to are many:

- Pedal-assist (“Pedelec”) ebikes
- Scooters
- Skateboards (although braking would have to be considered)
- Wheelchairs
- Bike trailers
- Dollies
- Handtrucks
- Wheelbarrows

## Prior Art

- A similar principle is used in trailer brake controllers. When the towing vehicle applies its brakes, an accelerometer is used to measure the braking rate, and a proportional signal is used to apply the brakes of the trailer. 
- An accelerometer-based system for a pedal assist ebike is described [here](https://ebikes.ca/learn/pedal-assist.html). 
- An alternative torque-sensorless assist system is described [here](https://www.jstage.jst.go.jp/article/ieejjia/6/2/6_124/_pdf).

[^1]: Less expensive options exist that measure the rider's cadence (pedal speed) and vary the motor's output accordingly, but they have a much less natural feel than systems based on a torque sensor. 
