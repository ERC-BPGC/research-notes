# BEECLUST: A Swarm Algorithm Derived from Honeybees.

<strong>Written By:</strong> Arjun Puthli 

Refer to paper [here](http://heikohamann.de/pub/schmickl_beeclust_2011.pdf)

---

## Biological Inspiration

The inspiration for this algorithm was how bees behave in environments with temperature gradients. Consider a pipe with one end being heated while the other is in a cold water bath. This creates a steep temperature gradient. Bees tend to aggregate towards warmer areas and group up in some optimum spot. Certain rules were created to model this behaviour. Due to sensor limitations on the bot, this environment was replicated using a light source.

## Algorithm Summary

The algorithm has a few general steps that it follows:

1. The bot moves in some random direction
2. If the bot encounters a wall, it moves away. We can model this behaviour in two ways
    * bot turns around and goes back the way it came or
    * bot chooses a new random point and goes there
3. If the bot encounters another robot, they stop in the same spot for a certain amount of time T. After time T has elapsed, the bots behave like they would in step 2.

The waiting time depends upon the brightness of the spot the bots are currently in. By setting some maximum waiting time, T can be calculated by the following formula:

$$
T(e) = \frac{T_{max}e^{2}}{e^{2}+7000}
$$

Where e is the sensor reading scaled linearly between 0 to 180, corresponding to the range of 0 lux to 1500 lux (The sensor is the one used on the Jasmine bot, hardware details can be found here [www.swarmrobot.org](http://www.swarmrobot.org/)).

## Compartment Model

Consider an arena with concentric circle zones, each with different light intensities. The zone right in the centre has the brightest intensity, and it gets dimmer as you go radially outwards. 

[]()

**Robot Diffusion:** Since robots move from one zone to another, the rate at which they flow can be represented by $\delta_{ij}$ where the robots are moving from zone i to zone j.

**Change of State:**  When a robot encounters another robot the robot stops for a certain amount of time. The rate at which this change of state occurs is represented by $\alpha_i$. The bots start moving again after a certain amount of time elapses, and the rate at which this occurs is represented by $\beta_i$

The rate at which robots move from zone I to I-1 can be written as:

$$
\delta_{i,i-1}(t) = 0.5vDF_i(t)\frac{R_{i-1}}{(R_i + R_{i-1})(R_i - R_{i-1})}
$$

Where $F_i(t)$ is the number of free roaming robots in the zone, D = 0.5 is the diffusion coefficient and v is the average velocity of the bots. Since the outermost zone has only a single neighbour and is shaped slightly differently, the diffusion of bots from zone 5 to zone 4 can be modelled as:

$$
\delta_{5,4}(t) = \frac{vDF_5(t)}{l_{arena}- R_4}
$$

The robots also have a certain likelihood of recognising another bot as the arena wall, or another robot. Let $P_{detect}$ be the probability that a bot recognizes another bot as a bot and not a wall, and let $z_i$ represent the area of the focal zone $i$. $C_{f,f}$  is the likelihood of two free moving robots colliding with each other, while $C_{f,a}$   is the likelihood of a freemoving robot coliding with a stationary robot. The rate at which the robots collide and aggregate together can be modelled as:

$$
\alpha_i(t) = F_it(t)P_{detect}\frac{C_{f,a}A_i(t) + C_{f,f}F_i(t)}{Z_i}
$$

The rate at which stationary robots start to move again is represented by $\beta_i$ for each zone $i$. The waiting time can be approximated by $\frac{1}{w_i}$ where $w_i$ is the average waiting period of the robots in zone $i$.

According to the Jasmine bot’s hardware, the luminance sensor maps $E_i$ between 0 and $l_{max} =$  1500 lux in a linear manner. For this range of luminance, the sensor reports a value between 0 and 180, therefore:

$$
e_i = min(180,256\frac{E_i}{l_{max}})
$$

The average waiting time modelled on the sensor output is (where $w_{max}$ is the maximum waiting time):

$$
w_i = max(1, \frac{w_{max}e_i^{2}}{e_i^{2}+7000}) 
$$

If $A_i(t)$  is the number of robots aggregated together in a zone, then the rate at which stationary bots start moving again is:

$$
\beta_i(t) = \frac{A_i(t)}{w_i}
$$

The above equations can be merged into an ODE to represent the rate of change of free moving robots in a zone $i$:

$$
\frac{dF_i(t)}{dt} = \delta_{i-1, i}(t) + \delta_{i+1, i}(t) - \delta_{i, i-1}(t) - \delta_{i, i+1}(t) - \alpha_i(t) + \beta_i(t)
$$

For the right-most compartment $i=1$ we set $\forall$ t    
$\delta_{i-1,i}(t) = \delta_{i,i-1}(t) = 0$

The number of aggregated robots in each zone is:

$$
\frac{dA_i(t)}{dt} = \alpha_i(t) - \beta_i(t)
$$

## Macroscopic Model

In this approach, instead of taking separate compartments/zones, continuous space is considered by using partial differential equations. The approach considered by the authors was making use of Brownian motion. The change in a particle’s position X can be described by Langevin’s equation under certain assumptions:

$$
\frac{dX}{dt} = -Q + DW
$$

Where Q is the drift, D is the diffusion and W is a stochastic process. Using the Kolmogorov forward equation ($P'(t) = P(t)Q$, where P(t) is the probability matrix and Q is the generator).

The Kolmogorov forward equation corresponding to the above equation is:

$$
\frac{\partial \rho(x,t)}{\partial t} = -\nabla(Q\rho(x,t)) + \frac{1}{2}\nabla^{2}(D^{2}\rho(x,t))
$$

$\rho(x,t)d_{x}d_{y}$  is the probability of encountering the particle at point x within a rectangle defined by dx and dy at time t. The above equation can be simplified by only considering diffusion:

$$
\frac{\partial \rho(x,t)}{\partial t} =  \frac{1}{2}\nabla^{2}(D^{2}\rho(x,t))
$$
