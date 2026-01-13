# Textual Counterfactual Explanations

<div style="font-size:0.85em">

How can we generate counterfactual explanations for text? The challenge is different from tabular or image data – text changes must preserve linguistic fluency while achieving the desired classification change.

**Approach:** We leverage large language models to generate minimally modified text that flips a classifier's prediction.

For example, given a negative movie review like "This movie was boring", we want the LLM to produce a minimal edit that makes a sentiment classifier predict positive, such as "This movie was quite interesting" rather than adding excessive modifications.

The key insight is that we can use the classifier itself to automatically generate training data through preference pairs, then fine-tune the LLM to consistently produce high-quality counterfactuals.

</div>

<SlideNumber/>

---

# Automatic Preference Data Collection

<div style="font-size:0.8em">

We collect preference pairs automatically using the classifier as a judge:

<br>

| **Input** | **Candidate A** | **Candidate B** | **Preferred** |
|-----------|-----------------|-----------------|---------------|
| "This movie was boring." | "This movie was *quite interesting*." | "This movie was *boring and dull*." | A |
| "The weather was awful." | "The weather was amazing." | "The weather was awful!" | A |

<br>

**Labeling criterion:** We prefer whichever candidate achieves the target classifier label with minimal edit distance from the original.

This becomes a training example: $(x, y_{\text{good}}, y_{\text{bad}})$

</div>

<SlideNumber/>

---

# Data Collection Process

<div style="font-size:0.8em">

Given a classifier $f$ and a base sentence $x$, we systematically generate preference pairs:

1. **Generate candidates:** Ask the LLM for $K$ candidate edits of $x$ toward target class $c^*$
2. **Score each candidate:**
   $$
   s(y) = f_{c^*}(y) - \lambda_d \, d(x, y)
   $$
   where $f_{c^*}(y)$ is classifier confidence and $d(x, y)$ is edit distance
3. **Select pairs:** Take the top and bottom scoring candidates as $(y^+, y^-)$

The scoring function balances two objectives: achieving high classifier confidence for the target class while minimizing changes to the original text.

We can gather tens of thousands of such pairs automatically, creating a rich training dataset without manual annotation.

</div>

<SlideNumber/>

---

# Training with Direct Preference Optimization

<div style="font-size:0.75em">

We use the preference pairs to fine-tune the LLM using Direct Preference Optimization (DPO).

Let $\pi_\theta(y|x)$ be the current policy (LLM) and $\pi_\text{ref}(y|x)$ be the frozen reference model. The DPO loss is:

$$
L_{\text{DPO}}(\theta)
= -\,\mathbb{E}_{(x,\,y^+,\,y^-)}\!
\left[
\log \sigma\!\left(
\beta \left[
\left(\log \pi_{\theta}(y^+|x) - \log \pi_{\theta}(y^-|x)\right)
-
\left(\log \pi_{\text{ref}}(y^+|x) - \log \pi_{\text{ref}}(y^-|x)\right)
\right]
\right)
\right]
$$

where:
- $y^+$: preferred (good) counterfactual
- $y^-$: rejected (bad) counterfactual
- $\beta$: temperature controlling how strongly preferences shape the policy

This loss increases the relative log-likelihood of preferred outputs compared to rejected ones, while keeping the model close to the reference. The result is an LLM specialized in generating minimal, valid textual counterfactuals.

</div>

<SlideNumber/>
