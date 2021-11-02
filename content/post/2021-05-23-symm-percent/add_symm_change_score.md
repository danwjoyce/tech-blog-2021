---
title: "Alternatives to Percentage Change"
date: 2019-04-18T19:47:56+01:00
draft: false
categories: [change, trajectories]
tags: [measures]
---

The motivation for looking at this topic was primarily my naivety and
getting counter-intuitive results when I was looking at serial measures
of response to treatment. In particular, I wanted to use outcome
measures based on published conventions in the literature for threshold
response (the problems with this approach are a topic for another day).
I found some pointers on Frank Harrell's blog on the topic of
[percentages](http://www.fharrell.com/post/percent/), which mirrored
some of the discussion in Chapter 8 of (Senn 2008) which looks at
assumptions of additvity in treatment effects.

Here's the context in clinical trials in psychiatry: To define a
clinically-meaningful outcome (e.g. to a treatment/intervention) the
literature frequently uses *percentage change from baseline* -- for
example (Leucht et al. 2009; Munk-Jørgensen 2011) in the context of
PANSS scores in psychosis. I'll avoid the debate about why this may not
be appropriate for statistical modelling (e.g. using change from
baseline severity rather than using the baseline as a covariate in a
statistical model).

To make this concrete, let A be the first measurement, and B the second
(i.e. pre-treatment and post-treatment respectively, or baseline and
endpoint). Leaving aside the correction to ensure minimum symptoms
severity is 0, Leucht's formula for **percentage reduction from baseline
PANSS** is essentially:

$$
100 \\times \\frac{A - B}{A}
$$

To make the example concrete, one of the criteria for defining
treatment resistance in schizophrenia (Howes et al. 2016) is: "**less
than 20% symptom reduction during a prospective trial**" (of treatment)
using Leucht *et al*'s formula (above). Re-wording this (to make it
compatible with Leucht): failure of a treatment trial is that the
**percentage reduction from baseline is less than 20%**.

Percentages
===========

The obvious reason for favouring percentage change from baseline is that
improvement (or worsening) is relative the patient's initial level of
symptom severity. Assuming higher scores represents higher symptom
burden, a patient moving from a baseline A = 97 to an endpoint B = 77
represents a 20% change for that patient. A patient starting from a more
modest symptom burden, say A = 65, improving to B = 52 similarly
represents a 20% improvement.

However, percentages are not **symmetric** -- for example, if the first
measurement is A = 97, and the second (post-treatment) improved score is
B = 67 we have a **percentage reduction** of
$$ 
100 \\times \\frac{97-67}{97} \\approx 31 \\% 
$$

Switching this around, for another patient who gets **worse** by exactly
the same absolute amount after treatment: A = 67 and B = 97, yields
$$ 
100 \\times \\frac{67-97}{67} \\approx -45 \\%
$$

Notice that both the sign and magnitude of the change are different,
but the absolute change in units is 30 in both cases.

The next problem -- percentages are not **additive**; assume a 30%
**improvement** from an initial score of A = 100 -- the endpoint
(post-treatment) would be B = 70:

$$
100 \\times \\frac{100 - 70}{100} = 30\\%
$$

Now assume we follow the same patient from B to another time point, C,
and they've gotten worse, returning to their baseline of 100:
$$
100 \\times \\frac{70 - 100}{70} \\approx -42.9\\%
$$

On absolute scale units we have a series of measurements A = 100, B =
70 and C = 100, but we have seen a percentage change of 30% (A to B)
followed by a percentage change of -42.9% (B to C) despite A = C. In
other words, if we take the baseline A, add the change from A to B and
the change from B to C, we should have **zero net change** from the
baseline, A to the last measurement C:

$$
\\begin{aligned}
A + (A-B) + (B-C) &= 100 + (100 - 70) + (70 - 100) \\\\\\
                  &= 100 + 30 - 30 \\\\\\
                  &= 100
\\end{aligned}
$$
 But if we use percentages, this is not the case; assuming that A
represents 100% -- the baseline level of symptom severity -- and abusing
notation, we let (A-B) stand for the percentage change from A to B, and
similarly for (B-C):
$$
\\begin{aligned}
A + (A-B) + (B-C) &= 100 + 30 - 42.9 \\\\\\
                  &= 87.1\\%
\\end{aligned}
$$
 Percentage change from baseline is intuitive because it allows for a
familiar and uniform representation of change *relative* to the
patient's baseline, but it's counter-intuitive because it is asymmetric
and non-additive.

Here's a graphical representation: let the baseline (A) and endpoint (B)
values range from 0 to 100 respectively -- the interactive graph below
shows the behaviour of percentage change as both A and B vary:

<iframe width="900" height="800" frameborder="0" scrolling="no" src="//plot.ly/~danwjoyce/11.embed"></iframe>

Sympercent
==========

The idea proposed in (Cole 2000), neatly illustrated in (T. J. Cole and
Altman 2017b; T. J. Cole and Altman 2017a) is to exploit a property of
natural logarithms where because
$$
\\ln(A)−\\ln(B)=\\ln(A/B)
$$
the ratio A divided by B can be expressed as a sum, retaining
additivity and symmetry, and it turns out, approximating a 'percentage'
change. The proposed name for this measure is "sympercent" and (Cole
2000) gave it the symbol **s%** and the formula
$$
100 × \[\\ln(A)−\\ln(B)\]
$$
Repeating the examples above; first of all A = 97, B = 67 which gave
us a 31% (percent) improvement instead gives us the sympercent change:

$$
100 × \[\\ln(97)−\\ln(67)\] ≈ 37 s\\%
$$

A different numerical value (37, versus 31). Note, however, that we
gain symmetry -- so with A = 67 and B = 97 (worsening symptoms from A to
B):

$$
100 × \[\\ln(67)−\\ln(97)\] ≈ −37 s\\%
$$

The magnitude is the same (37) but the sign changes to represent
worsening, rather than improvement.

Repeating the additivity example; improvement from A = 100 to B = 70 but
with subsequent worsening back to C = 100. We'll drop the factor of 100
to simplify presentation:
$$
\\begin{aligned}
\\ln(A) + \\big\[ \\ln(A)-\\ln(B) \\big\] + \\big\[\\ln(B)-\\ln\(C\) \\big\] &= 4.61 + 0.36 - 0.36  \\\\\\
                             &= 4.61
\\end{aligned}
$$

That is, a net change of zero from the baseline.

Switching to an alternative representation affords a more interpretable
and intuitive measure of change that preserves the idea of change
**relative** to the patient's baseline.

Below, the behaviour of sympercent is shown over the range \[0,100\],
analogous to the percent change shown above:

<iframe width="900" height="800" frameborder="0" scrolling="no" src="//plot.ly/~danwjoyce/13.embed"></iframe>

The Original Problem
====================

Let's apply the 'sympercent' idea for the problem originally introduced
-- a threshold of 20% improvement for symptoms.

To simplify presentation, we drop the factor 100 and instead, work in
fractions of 1 (i.e. rather 20% we say 0.2 or 1/5). If we require at
least a 20% reduction in symptom scores to qualify as a response, then
we are saying:

$$
\\begin{aligned}
  \\frac{A-B}{A} &= 0.2 \\\\\\
  A-B &= 0.2 \\times A
\\end{aligned}
$$
 i.e. the difference between symptom scores at time points A and B
should be a fraction (0.2, or one-fifth) of the baseline A:

Reusing our first example, where A = 97:
$$
\\begin{aligned}
  97-B = 0.2 \\times 97
\\end{aligned}
$$
 So, for symptoms to have improved by at least 20% from a baseline of A
= 97, the symptom score at B should be 77.6 or lower.

What would this reduction look like in s% (sympercents)? 

If A = 97, and we know a 20% reduction (or 0.2) would represent B = 77.6 then:

$$
100 × \[\\ln(97)−\\ln(77.6)\] ≈ 22.3 s\\%
$$

References
==========

Cole, Tim J, and Douglas G Altman. 2017a. “Statistics Notes: Percentage
Differences, Symmetry, and Natural Logarithms.” *BMJ* 358. British
Medical Journal Publishing Group: j3683.

———. 2017b. “Statistics Notes: What Is a Percentage Difference?” *BMJ*
358. British Medical Journal Publishing Group: j3663.

Cole, TJ. 2000. “Sympercents: Symmetric Percentage Differences on the
100 log<sub>*e*</sub> Scale Simplify the Presentation of Log Transformed
Data.” *Statistics in Medicine* 19 (22). Wiley Online Library: 3109–25.

Howes, Oliver D, Rob McCutcheon, Ofer Agid, Andrea De Bartolomeis, Nico
JM Van Beveren, Michael L Birnbaum, Michael AP Bloomfield, et al. 2016.
“Treatment-Resistant Schizophrenia: Treatment Response and Resistance in
Psychosis (Trrip) Working Group Consensus Guidelines on Diagnosis and
Terminology.” *American Journal of Psychiatry* 174 (3). Am Psychiatric
Assoc: 216–29.

Leucht, S, JM Davis, RR Engel, W Kissling, and JM Kane. 2009.
“Definitions of Response and Remission in Schizophrenia: Recommendations
for Their Use and Their Presentation.” *Acta Psychiatrica Scandinavica*
119. Wiley Online Library: 7–14.

Munk-Jørgensen, Povl. 2011. “Corrigendum.” *Acta Psychiatrica
Scandinavica* 124 (1): 82–82.
doi:[10.1111/j.1600-0447.2011.01720.x](https://doi.org/10.1111/j.1600-0447.2011.01720.x).

Senn, Stephen S. 2008. *Statistical Issues in Drug Development*. Vol.
69. John Wiley & Sons.
