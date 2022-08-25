This repository contains the code for an agent-based simulation of the
relationship between individual metacognition and social information use
(i.e. social influence).

Our simulation takes inspiration from the work by @Rollwage2021. The
difference is that instead of modeling individual integration of
secuential information, we model the integration of social and
individual-specific information.

For our simulation we try to model the experimental task described by
@Voss2008 and employed (albeit modified) by @Germar2014 in the context
of a social influence experiment. Briefly, this task presents a square
matrix colored with two different colors in varying proportions while
the subject task is to decide which color is predominant. The matrix
size, colors, and proportions vary within the literature. @Voss2008
reports using “Squares of 200 × 150 screen pixels” while @Germar2014
reports “Squares of 128 × 128 orange and blue pixels were presented at a
resolution of 512 × 512 screen pixels”. As different colors, some
authors use orange and blue \[@Germar2014\], while others use orange,
green, or blue. Lastly, from the perspective of one color, @Voss2008
reports using 56%, 53%, 50%, 47%, or 44% proportions, while @Germar2014
employs 48%, 49%, 50%, 51%, 52%.

Since only one paper \[@Germar2014\] that we are aware of has given
detailed numerical summaries of performance with this task (in a control
situation), we use those values as a reference for subject performance.
In the following table, we present the aforementioned results.

Following @Rollwage2021, we model a subject reciving one sample of
information per trial denoted as
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

Thus, for one of the proportions given in @Germar2014 we would have
something like *X*<sub>*i*</sub>*n**d* ∼ 𝒩(0.48,0.025). The common mean
(*μ*) corresponds to the actual world state\[@Rollwage2021\] (i.e. the
correct color proportion) and the sampled value corresponds to
perceptual information (in a continuum) that defines the binary
decision. We formalize each agent’s decision as

$decision(x\_{ind}) = \left\\{\begin{array}{lr}1 & \text{if } x\_{ind} \geq 0.5 \\\\ 0 & \text{if } x\_{ind}\leq 0.5\end{array}\right\\}$

where *x*<sub>*i**n**d*</sub> is a realization of the random variable
*X*<sub>*i**n**d*</sub>, 1 represents some arbitrary color and 0
represents its complement color.

Confidence in the individual decision is estimated by the log-odds in
favour of the chosen world state \[@Rollwage2021\] as

$LO\_{ind}= \frac{2\mu X\_{ind}}{\sigma^2}$ which may be, for example,
$LO_i= \frac{2(0.48) X\_{ind}}{0.025}$.

Using the logistic function the log-odds can be transformed into the
probability of answering correctly \[@Rollwage2021\]

$confidence\_{ind} = \frac{e^{LO\_{ind}}}{1+e^{LO\_{ind}}}$.

After the initial decision, the agent receives additional information in
the form of answers given by other agents modeled as previously
described. Nonetheless, we can manipulate the other agent’s accuracy by
sampling sigma from a distribution. Thus, in the simplest case all
agents have the same *σ*<sup>2</sup>, which means that the precision
$\lambda = \frac{1}{\sigma^2}$ of their information is the same. In
another scenario, the precision of each agent depends upon a random
process as *σ*<sup>2</sup> ∼ *I**n**v**G**a**m**m**a*(*α*,*β*) where the
mean is fixed and defined $\frac{\beta}{\alpha - 1}=0.025$. Since there
are potentially
