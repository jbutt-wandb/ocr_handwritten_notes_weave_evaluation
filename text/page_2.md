- A probability path is a continuous sequence of distributions from $t=0$ to $t=1$, denoted $\{p_t\}_{t=0}^1$. Each $p_t$ is a probability distribution.

- For a fixed pair $(\mathbf{x}_0, \mathbf{x}_1)$, flow matching aims to move a noise point $\mathbf{x}_0$ along a straight line until it arrives to $\mathbf{x}_1$:

$$
\mathbf{x}_t = (1-t)\mathbf{x}_0 + t\mathbf{x}_1
$$

At $t=0$, $\mathbf{x}_t = \mathbf{x}_0$ and we have Gaussian noise.

At $t=1$, $\mathbf{x}_t = \mathbf{x}_1$ we have a real data sample.

At $0 < t < 1$, $\mathbf{x}_t$ lies along the line segment connecting $\mathbf{x}_0$ and $\mathbf{x}_1$.

- If we average over all noise-data pairs, the distribution of $\mathbf{x}_t$ interpolates between $p_0$ and $p_1$.

- If we differentiate along the line segment:

$$
\frac{d\mathbf{x}_t}{dt} = -\mathbf{x}_0 + \mathbf{x}_1 = \mathbf{x}_1 - \mathbf{x}_0
$$

- More formally, ...