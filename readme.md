# Simulation Summary

This repository contains the code for an agent-based simulation of the
relationship between individual metacognition and social information use
(i.e. social influence).

Our simulation takes inspiration from the work by Rollwage and Fleming
(2021). The difference is that instead of modeling individual
integration of secuential information, we model the integration of
social and individual-specific information.

For our simulation we try to model the experimental task described by
Voss, Rothermund, and Brandtstädter (2008) and employed (albeit
modified) by Germar et al. (2014) in the context of a social influence
experiment. Briefly, this task presents a square matrix colored with two
different colors in varying proportions while the subject task is to
decide which color is predominant. The matrix size, colors, and
proportions vary within the literature. Voss, Rothermund, and
Brandtstädter (2008) reports using “Squares of 200 × 150 screen pixels”
while Germar et al. (2014) reports “Squares of 128 × 128 orange and blue
pixels were presented at a resolution of 512 × 512 screen pixels”. As
different colors, some authors use orange and blue (Germar et al. 2014),
while others use orange, green, or blue. Lastly, from the perspective of
one color, Voss, Rothermund, and Brandtstädter (2008) reports using 56%,
53%, 50%, 47%, or 44% proportions, while Germar et al. (2014) employs
48%, 49%, 50%, 51%, 52%.

Since only one paper (Germar et al. 2014) that we are aware of has given
detailed numerical summaries of performance with this task (in a control
situation), we use those values as a reference for subject performance.
In the following table, we present the aforementioned results.

Following Rollwage and Fleming (2021), we model a subject reciving one
sample of information per trial denoted as
*X*<sub>*i**n**d*</sub> ∼ 𝒩(*μ*,*σ*<sup>2</sup>), which is the
information sampled by individual. Within this experimental task, *μ*
always equals the proportion for a fixed color (like blue) and therefore
remains the same for all subjects under the same condition. Moreover
this value usually varies within values like .48 or .52 since values
$$0.60 and $$0.40 are too easy for subjects given previous empirical
evidence. On the other hand, *σ*<sup>2</sup> remains the same for all
conditions but can be common to all subjects or vary in a
subject-specific manner (noted, *σ*<sub>*i*</sub><sup>2</sup>) following
a given random distribution. As a first approach we set
*σ*<sup>2</sup> = 0.025 (you can check the `pilot` folder for details on
this number).

Thus, for one of the proportions given in Germar et al. (2014) we would
have something like *X*<sub>*i**n**d*</sub> ∼ 𝒩(0.48,0.025). The common
mean (*μ*) corresponds to the actual world state(Rollwage and Fleming
2021) (i.e. the correct color proportion) and the sampled value
corresponds to perceptual information (in a continuum) that defines the
binary decision. We formalize each agent’s decision as

$decision(x\_{ind}) = \left\\{\begin{array}{lr}1 & \text{if } x\_{ind} \geq 0.5 \\\\ 0 & \text{if } x\_{ind}\leq 0.5\end{array}\right\\}$

where *x*<sub>*i**n**d*</sub> is a realization of the random variable
*X*<sub>*i**n**d*</sub>, 1 represents some arbitrary color and 0
represents its complement color.

Confidence in the individual decision is estimated by the log-odds in
favour of the chosen world state and using the logistic function the
log-odds can be transformed into the probability of answering correctly
(Rollwage and Fleming 2021).

After the initial decision, the agent receives additional information in
the form of answers given by other agents modeled as previously
described. Nonetheless, we can manipulate the other agent’s accuracy by
sampling sigma from a distribution. Thus, in the simplest case all
agents have the same *σ*<sup>2</sup>, which means that the precision of
their information is the same. In another scenario, the precision of
each agent depends upon a random process (modeled by an inverse gamma
distribution) as *σ*<sup>2</sup> ∼ *I**G*(*α*,*β*) where the mean is
fixed at 0.025. Since there are potentially infinite values of *α* and
*β* with the same mean, we vary over different combinations of
parameters that satisfy the above condition.

Germar, Markus, Alexander Schlemmer, Kristine Krug, Andreas Voss, and
Andreas Mojzisch. 2014. “Social Influence and Perceptual Decision
Making: A Diffusion Model Analysis.” *Personality and Social Psychology
Bulletin* 40 (February): 217–31.
<https://doi.org/10.1177/0146167213508985>.

Rollwage, Max, and Stephen M. Fleming. 2021. “Confirmation Bias Is
Adaptive When Coupled with Efficient Metacognition.” *Philosophical
Transactions of the Royal Society B* 376 (April).
<https://doi.org/10.1098/RSTB.2020.0131>.

Voss, Andreas, Klaus Rothermund, and Jochen Brandtstädter. 2008.
“Interpreting Ambiguous Stimuli: Separating Perceptual and Judgmental
Biases.” *Journal of Experimental Social Psychology* 44 (4): 1048–56.
