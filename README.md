# Assignment 2: Stochastic and Spatial Models
## Introduction
In this assignment you (in teams of two people) will be exploring other ways to model
infectious diseases. In the first part of the assignment you will use a stochastic discrete
event model to compute the spread of an infectious disease through a population. And in
the second half of the assignment you will explore spatial models (in particular networks)
to study the spread of infectious diseases.

## Problem 1: Gillespie’s Direct Algorithm and Stochastic Hallmarks
You will investigate the five hallmarks of stochastic SIR dynamics using an event drive
SIR model:
1. variability;
2. negative co-variances;
3. increased transients;
4. stochastic resonance;
5. extinctions.

### Implement Gillespies algorithm
Write some python code to implement Gillespies Algorithm (GA). [^1] You should define the 
events and the rates of each event for the SIR model. Keep in mind it may be insightful 
to compare the GA stochastic simulation with an equivalent deterministic ODE model.

BONUS: You can also think about (and implement) a way to control the noise level in
the GA.

[^1]: You can use first reaction, direct, or other GAs

### Investigate Simulation Variability and Negative Co-variance
In the first experiment you should investigate how varying the model parameters changes
the behaviour of the stochastic dynamics, in particular how they relate to variance between
runs and how they impact negative covariance between S and I. Compare the mean of
the stochastic simulations with the equivalent deterministic model output (do this for
multiple settings of the model parameters).

### Stochastic Resonance and Increased Transients
Show how the stochastic model can induce stochastic resonance around the equilibrium 
and how that resonance relates the model parameters (e.g., $N, \beta$, etc). Show some
examples of increased transients away from the deterministic equilibrium - can you show
which parameter values lead to the largest transients.

### Extinction events and Critical Community Size
In the lectures we have discussed the possibility of extinction of the virus even when
$R_0 > 1$ in closed populations. Design an experiment to show how varying R0 impacts
the extinction process. Keep in mind that in the closed system randomness will always
eventually lead to extinction. Now look at how the extinction events are impacted by
the population size. Find a way to show how the two parameters R0 and N interact to
impact the extinction process.



## Problem 2: Spatial Models - Networks
In this question you are asked to develop a set of experiments to design and evaluate
vaccination strategies using a network model. Using the package NDLib[^2] you should assess
the spread of a disease (SIR) across different types of model networks (Barabasi Albert,
Watts-Strogatz, Erdos-Reyni). Finally, you will run a simulated vaccination campaign
on a real contact network collected by sociopatterns (link). A modified version of this
dataset can be downloaded from Canvas, the network has been converted to a static
(non-temporal) form and some edges and nodes have been filtered out.

[^2]: Note there are SIR Network implementations/examples that you can re-use/adapt in the
library see https://ndlib.readthedocs.io/en/latest/tutorial.html

### Implement SIR and Simulate
Implement SIR disease spread on the network. Think about your experimental design
and which parameters of the model you will want to vary. Design code that will allow
you run multiple simulations while varying the disease parameter.

### Generate Networks of equivalent form
Using NetworkX generate multiple model networks with similar characteristics, again
think about the parameters associated with each network generator (e.g., Number of
nodes, connection probability,etc). Pick some network statistics (e.g., centrality measures,
degree distributions, etc.) that are interesting to measure in terms of spreading on the
network. You should generate multiple instances of each network type and then plot the
network statistics (you chose) and discuss how these statistic differ between network types
and for different parameter settings. You will use these generated networks in your SIR
experiments in the next part.

### Simulate SIR spread on the network
Simulate epidemic spreading on the networks you generated in the previous section
(NOTE: the simulations will be stochastic so think about random seeds and repitions).
You can vary the fraction of initial infected, which nodes are initially infected and other
disease parameters. Compare and discuss how the disease spreads in the different networks
under different conditions.

### Dynamic Vaccination Campaign
Finally, we will conduct vaccination experiments using the sociopatterns dataset, see
Figure 1. You should design some code to load in the sociopatterns dataset (this file on
canvas includes a simple edgelist and NDLib and NetworkX provide ways to import this).

![alt text](media/readme/datasetView.png)

**Figure 1**: A view of the filtered sociopatterns data set with 374 nodes and 1265 edges. The
nodes are people and edges exist if the two people spent time near each other during the
conference. Note that in the original dataset edges had weights indicating how long those
people were in range of one another. In this assignment we disregard the weights and
assume all contacts involve equal length of contact and hence equal chance of transmission.

Now consider a scenario in which a disease is spreading on this network. You should run
multiple experiments, but assume that the disease always starts with a random selection
of 5 nodes infected.

You are to design a dynamic vaccination strategy in which you have a testing budget and
a limited number of vaccinations available per iteration of the model. Assume that you
have 200 tests in total, you can use a maximum number of tests per iteration (this will
vary per experiment see below), you can of course use less. You can use the tests at any
point during the spread and you may repeat tests on a node as often as you like. You
can assume that you know the network structure, but you can only discover the disease
status of a node after a test. Vaccinations can only be applied to susceptible people and
that they immediately move people to the removed state. Finally, you might consider
situations where the tests are not always accurate, but instead have some probability
(which you can vary) of being accurate. You can also assume that people remain removed
until the end of the simulation (no waning immunity).
You should compare your strategy against a simple null strategy which randomly assigns
vaccinations, you should design a strategy that at least out performs the null strategy.
Compare the strategies with different vaccination budgets of [1, 3, 5 and 10] per timestep
and compare with different testing accuracy [0.5, 0.75, 1.0] . Finally, keep in mind that
the purpose of the assignment is not to design the best strategy, but to evaluate you
strategy in a systematic and scientific manner.