# Making Our Own Data

We're going to make some fake astronomy data!  Recall from the previous day that when a planet crosses in front of its host star (from our point of view), the brightness dips.  We're going to learn how to come up with our own solar systems!

What I show you here will allow you to go crazy and add in as many planets as you want!  But first, we're going to make our own "tool" (also called a function) that we can use add in our planets to our fake data.

## Math Review

Let's review some of the math behind exoplanet transits. Recall from before, that the change in brightness of the star during a transit is related to the apparent areas of the planet and star:

```{math}
:label: flux-ratio2
\frac{\Delta F}{F} = \frac{\pi R_{p}^2}{\pi R_{s}^2} = \left( \frac{R_{p}}{R_{s}} \right)^2
```

The greek letter $\Delta$ means "change" in whatever quantity that follows. In other words, $\Delta F = F_{initial} - F_{final}$. Remember that we normalize flux values so that the usual value is $1.0$. Therefore, $F_{initial}=1$ in our case of a transiting planet. 

How long does a transit last for? The math here gets complicated, but through some geometry the formula for solving for this quantity is as follows:

```{math}
:label: lc-duration
t_{duration} = \frac{P}{\pi}\sin^{-1} \left( \frac{\sqrt{(R_s+R_p)^2-(bR_s)^2}}{a} \right)
```

where $P$ is the period, $b$ is the impact parameter, and $R$ is radius of the planet or star. If you want to see the derivation of this equation, read this: https://www.paulanthonywilson.com/exoplanets/exoplanet-detection-techniques/the-exoplanet-transit-method/

For our program today, we will choose the periods of our exoplanets, and have Python solve for the semimajor axis of the orbits. Recall from the previous day that the semimajor axis can be found by rearranging Kepler's third law (Equation {eq}`kepler-third-period`), and we found that

```{math}
:label: kepler-third-semimajor2
a = \left( \frac{P^2 G M_s}{4\pi^2} \right)^{1/3},
```

where $a$ is semimajor axis, $G$ is the gravitational constant, and $M$ is mass of the star or planet.
