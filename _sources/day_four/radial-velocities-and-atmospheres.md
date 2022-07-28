---
jupytext:
  cell_metadata_filter: -all
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Radial Velocities and Atmospheres

````{admonition} Question 1
:class: tip
How do you think you would go about finding the mass of a planet? How do you think we've calculated how much our Earth weighs (i.e. how much mass it has)? Think about what information we would need to be able to do that.

```{dropdown} How to find mass
One way to calculate the mass of a planet in a solar system outside of our own, is to look at radial velocity variations.
```
````

## Radial Velocities

When we talk about orbits in a solar sytem, how do we determine who oribts who? In our solar system, we think of it as pretty obvious that our Sun stays still while the planets orbit around it. But what if our Sun was only a few percent bigger than Jupiter: would our Sun still remain put while Jupiter goes around it, or would they orbit each other?

The fact is that no matter how large our Sun was, it is still "orbiting" around the planets, which in turn orbit the Sun. All of these celestial bodies are orbiting around each other about their center of mass, called the "barycenter" of the system. In the image below, the red cross indicates the barycenter of the system of two objects, one large and one small. Note that the barycenter lies at all times inside the interior of the larger body. The closer this barycenter coincides with the center of the larger object, the less it will jostle around.

```{image} https://upload.wikimedia.org/wikipedia/commons/5/59/Orbit3.gif
---
align: right
---
```

Often, this method for detecting planets known as **astrometry**, is rather difficult as changes in stellar positions are usually too small. Not even the best ground-based telescopes can measure stellar changes in position this small, since atmospheric distortions are too large relative to the required precision. As a result, until now no exoplanet has been discovered using this technique. (But it should be noted that the *Hubble Space Telescope* has confirmed, through astrometry, the existence of a previously-discovered planet in 2002, around a star known as Gliese 876. Also, *Gaia* is expected to detect thousands of planets through astrometry.)

The lack of success of this technique however does not mean that we aren't yet capable of detecting a star's motion due to the influence of an orbiting planet(s). This is where the **radial velocity** technique comes in. Just as you've learned in class, any vector can be broken into distinct components based on your coordinate system. The velocity vector of a star, for example, can be broken up in the same way. Any arbitrary motion of the star can be broken up into a component that's moving toward or away from you, the observer, and another componenent that's moving perpendicular to the line of sight. These motions are called, respectively, *radial velocity*, for the motion that changes in distance along your line of sight (i.e. its radius to you is changing), and the other called *proper motion*, for the motion that moves the star across the plane of the sky, as illustrated in the figure below.

```{image} https://upload.wikimedia.org/wikipedia/commons/f/f2/Proper_motion.JPG
:scale: 15%
```

```{admonition} Question 2
:class: tip
Sketch a graph of what you would imagine a star's radial velocity would like with time, assuming the plane of its orbit around the barycenter of the system it's in is oriented edge-on (i.e. the orbital angular momentum axis is perpendicular to your line of sight). 
```

```{admonition} Question 3
:class: tip
The following link will take you to an applet that shows what kind of radial velocity variations you'll observe when you vary certain parameters: http://astro.unl.edu/classaction/animations/extrasolarplanets/radialvelocitysimulator.html 

How does the graph of radial velocity change when you vary: inclination, stellar mass, planet mass, planet radius, and eccentricity of the orbit? What about if you vary the planet's radius (without touching anything else)? (This parameter's not an option in this applet, so you're going to have to think hard here.)
```

It turns out that the radial velocity variation of a star (let's assume it has just one planet orbiting it) as a function of time can be expressed in terms of its period $P$, orbital inclination $i$ (where $0^{\circ} < i < 90^{\circ}$) and semi-major axis $a$, its mass $M_{\ast}$, and the mass of the orbiting planet, $m_p$: 

```{math}
:label: exoplanet-mass
v_{r \ast}(t) = \left(\frac{2 \pi}{P}\right) \frac{m_p a}{M_{\ast}} \; sin(i) \; cos\left(\frac{2 \pi t}{P}\right)
```

```{admonition} Challenge
:class: warning
Using python, make a plot of the above radial velocity equation versus phase/period, for a given orbital inclination. See how this plot varies when you change the input value of $i$. (Your plot should resemble that in the link above.)  
```

```{code-cell} ipython3
import numpy as np
import matplotlib.pyplot as plt

G = 6.67e-11
P = 86400*860 # seconds
star_mass = 1.538*1.98e30 # kg
i = 0
planet_mass = 266*6.24e24 # kg
a = (P**2*G*star_mass/(4*np.pi**2))**(1./3.) # meters

t = np.linspace(0,86400*40,100)

v_r = (2*np.pi/P)*(planet_mass*a/star_mass)*np.sin(i)*np.cos(2*np.pi*t/P)
#plt.plot(t/86400,v_r)

v_r = v_r + np.random.normal(0,0.1,len(v_r))

f, ax = plt.subplots(dpi=150)

ax.plot(t/86400,v_r,'.k')

ax.set_xlabel('time (days)')
ax.set_ylabel('radial velocity (m/s)')
ax.set_xlim([np.min(t/86400),np.max(t/86400)])

print(np.max(v_r))
```

You might be asking yourself how it is that we measure these velocity variations. The answer lies within something called the **spectrum** of the light given off by the star. Since Newton's times, we've known that passing light through a prism will split it up into its consituent parts:

```{image} https://cdn.britannica.com/78/149178-050-F2421B64/light-prism-color-angle-colors-wavelength-wavelengths.jpg
:scale: 15%
```

Each of these constituent parts are split up as different colors, which correspond to different wavelengths of light. If the white light source happens to pass in front of some carbon-dioxide gas, for example, right before it hits the prism, you will notice some gaps or dark lines in your spectrum. In other words, your spectrum will be missing certain wavelengths of light. The reason is that the atoms in the gas the light passed through on its way to the prism (carbon dioxide atoms in this example) absorbed light of that particular wavelength and removed it from your light spectrum. Based roughly on this principle, we can tell what kinds of atoms make up the light a star is giving off by looking at their spectra (plural for spectrum). (See this link for a nice applet illustrating this point: https://www.khanacademy.org/computer-programming/interact-absorption-lines/2226438064)

See this video as well for more on spectroscopy in astronomy: https://www.khanacademy.org/partner-content/nasa/measuringuniverse/spectroscopy/v/spectroscopy-in-action

```{admonition} Question 4
:class: tip
How can we use light to tell how fast an object (e.g. a star) is moving toward or away from us? If you hold a super bright light (say, a 1000 lumen flash light) ten feet away and move it closer to or further from you, would you be able to tell?
```

You will have experienced by now that when a car is moving towards you at high speed it emits sound at a high-frequency, then when it passes you and starts moving away it will emit sound at a lower frequency. This is called **Doppler shift**. Watch this video for an example: https://www.youtube.com/watch?v=p-hBCcmCUPg

In the same way, light that is moving toward you gets shifted to higher frequencies (i.e. it gets blueshifted as a spectral line will move towards the bluer = higher-frequency side of the spectrum), and light that is moving away from you gets shifted to lower frequences (it gets "redshifted"). So if we look at the spectrum of a star, we can tell if it's moving away from us by looking at how the spectral lines move, and in turn, determine whether something in orbit around it is tuggin at it:

```{figure} https://lostintransits.files.wordpress.com/2014/08/rvgif2.gif
Credit: ESO
```

The spectrum shown in the above animation is simplified compared to what it really takes to precisely measure a stellar radial velocity. The following is a spectrum taken with the EXtreme PREcision Spectrograph (EXPRES):

```{image} static/expres_spectrum.png
:scale: 40%
```

```{image} static/EXPRES.jpg
:scale: 7%
```

The Doppler effect can give us the velocity of an object via the following formula:

```{math}
:label: doppler-velocity
\frac{v_0}{c} = \frac{\lambda - \lambda_0}{\lambda_0},
```

where $v_0$ is the velocity of the object, $c = 3 \times 10^8$ m/s is the speed of light, $\lambda$ and $\lambda_0$ are the observed and emitted wavelengths, respectively. 

```{admonition} Question 5
Now that we know how to read a radial velocity graph, and we know how radial velocity measurements are made, you are equipped to find the mass of an exoplanet. How? Look at the equation for radial velocity above. If you've looked at the transit light curve of a planet, you can calculate the period and semi-major axis of your planet's orbit, and the star's mass has been given to you (can you think of how you would get the mass of a star?). 
```

Let's use Kepler 435b from yesterday's activity as an example. To find the amplitude of the radial velocity variations of your star/planet system, (if these measurements exist for your system), go back to the find exoplanet database (https://exoplanetarchive.ipac.caltech.edu/cgi-bin/TblView/nph-tblView?app=ExoTbls&config=planets), find your star, and click to arrive at the confirmed planet overview page. Check some of the references to see if you can find radial velocity data for this star. Using the radial velocity equation and plot we made above, find the amplitude of the stellar radial velocities. Does your value agree with what is listed in the literature? Remember that, for Kepler 435b, the calculated/given values were as follows: P = 8.6 days, a = 0.09 AU, $M_p = 0.8 M_J$ and $M_{\ast} = 1.538 M_{\odot}$

As we can see, without knowing the orbital inclination $i$, we can only calculate a lower bound on the planet's mass, i.e. the minimum mass $m_{p,true}$sin$(i)$.

A planet may have an atmosphere, so that light passing from the host star will also produce spectral signatures unique to the planet, in addition to any thermal emission from the planet itself. If you can manage to separate a star's spectrum from the planet's spectrum, you can then measure the radial velocity of the planet as well, and thus find out the inclination of the orbit. Substituting this in turn into the expression for the minimum mass will allow you to then calculate the true mass of your planet. 

Obtaining the mass of a given exoplanet from the information derived from the radial velocity method, as well as the size the planet from transit light curve analysis, one can then estimate the average density of the planet, as well as its surface gravity. Knowing the density, one can then determine whether the planet is a gas giant, like Jupiter or Saturn, or a rocky planet, like Earth!