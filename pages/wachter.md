# Wachter's Method

<br>

The method is framed as gradient-based optimization that balances proximity to the original instance and achieving the desired prediction [@Wachter2017CounterfactualEW]:

$$
\mathbf{x}' = \arg\min_{\mathbf{x}'} \left[ \text{dist}(\mathbf{x}, \mathbf{x}') + \lambda \cdot \text{loss}(f(\mathbf{x}'), y') \right]
$$

where:
- $\text{dist}(\mathbf{x}, \mathbf{x}')$ measures the distance from the original instance,
- $\text{loss}(f(\mathbf{x}'), y')$ ensures the desired prediction $y'$,
- $\lambda$ controls the trade-off between the two objectives.

<SlideNumber/>

---

# Wachter's Method

<figure>
  <img src="../media/wachter.svg" style="width: 65%; display: block; margin: 0 auto;">
  <figcaption><FigureNumber/>Gradient-based search for counterfactual explanation. Visualization adapted from <a href="https://www.taija.org/CounterfactualExplanations.jl/stable/">CounterfactualExplanations.jl</a>.</figcaption>
</figure>

<SlideNumber/>
