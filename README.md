# Modelling_physical_laws

🚧 UNDER CONSTRUCTION 🚧

## Aims
The aim of this dissertation were to explore the application of neural networks in the field of modelling and discovering physical laws. According to the universal approximation theorem, a feedforward neural network with a specific structure can approximate any continuous function to an arbitrary degree of accuracy. I was interested in the following situation:

1. We have a neural network that perfectly models a given physical law- it has zero loss when compared to an Euler-Cromer method of the law.
2. We perturb the network's weights so that it no longer occupies a (hopefully global) minimum of loss.
3. In the situation that there were another law that was perfectly predictive based on the physical situation and data we have (IE it occupies another minimum in loss), would it be possible to train the network "onto" it and then gather information about the new law from the network's biases and structure?

In order to tackle the third point, we would first need to be able to return a perturbed network back to its original zero-loss state. We would also need the network to be structured in such a way that it is interpretable in terms of a closed-form equation. The "black box" nature of generic neural networks means that it can be particularly difficult to interpret how they create an output from inputs. If we can incorporate physics into the structure of the network, we may be able to "open" the black box and better understand how its weights may link to physics- this would be a so called physics-informed neural network (PINN).

So the goals of this study were to:

1. Construct and train a PINN that will perfectly predict the future state of a harmonic oscillator as dictated by the Duffing equation.
2. Find the limit of this NNs trainability from perturbed (and later randomised) weights.
3. Compare the performance of the PINN with that of a generic neural network approach.

## The Duffing Equation PINN

The Duffing equation is a non-linear second order ODE that models damped and driven oscillators.

$$
\ddot{x} + \delta \dot{x} + \alpha x + \beta x^3 = \gamma \cos(\omega t)
$$

$\delta$ controls the degree of damping, $\alpha$ controls the linear stiffness, $\beta$ adds non linearity to the restoring force, $\gamma$ controls the amplitude of the periodic driving force, and $\omega$ is the angular frequency of the periodic driving force. By setting $\beta=\gamma=0$, it models simple harmonic motion.

The training dataset for the PINN will consist of pairs $((f_{x_0},f_{y_0},z_0,v_0),(f_{x_1},f_{y_1},z_1,v_1))$. The system will be propogated via the following relations:

1. $f_{x_1} = cos(\omega t) f_{x_0}+sin(\omega t) f_{y_0}$
2. $f_{y_1} = -sin(\omega t)f_{x_0}+cos(\omega t)f_{y_0}$
3. $v_1=v_0+dt(-\alpha z_0 - \delta v_0-\beta z_0^3 +\gamma cos(\omega t))$
4. $z_1 = z_0+v_1 dt$


The diagram below demonstrates how the PINN will be structured. The red, blue, green, and white layers of this network represent steps 1, 2, 3, and 4 respectively.

<img width="955" height="971" alt="PINN diagram" src="https://github.com/user-attachments/assets/a62d16bd-b859-49dc-ae36-92e04ba645de" />


