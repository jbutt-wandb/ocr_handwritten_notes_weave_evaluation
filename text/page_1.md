## Morphing Probability distributions

- Let $p_0$ be a probability distribution on $\mathbb{R}^d$ that we can sample from. A good example is the Gaussian distribution
  $$p_0 = \mathcal{N}(\mathbf{0}, \mathbf{I}_d)$$

- Let $p_1$ be a data distribution, it can be the distribution of images or videos.

- To generate new images, we'd like to sample from $p_1$ but we don't know its analytic form. But we do have samples from $p_1$, $\{\mathbf{x}_i\}_{i=1}^N \sim p_1$.

## The Goal of Flow Matching

- Given a datapoint $\mathbf{x}$, sampled from a Gaussian, we want to learn a time varying vector field such that it informs us how much we should move $\mathbf{x}$ (at time $t$) via an offset vector $\Delta \mathbf{x}$.

- We want to move $\mathbf{x}$ to be closer to a point $\mathbf{x}'$ that could be sampled from a target distribution - in our case $p_1$.

- The goal of flow matching is to learn this time varying vector/velocity field from data

- Geometrically, imagine a random scattering of points in $\mathbb{R}^d$. These look like they've been sampled from a Gaussian. We want to move these points, on a time schedule, such that at the end of this process we have a different topology of points.

- If we have trained well, the topology of data points resemble a sample from $p_1$.