# Training Loop

- Given a minibatch of real data
1. Sample real data: $\mathbf{x}_1 \sim p_{1}$
2. Sample noise: $\mathbf{x}_0 \sim \mathcal{N}(\mathbf{0}, \mathbf{I})$. $\mathbf{x}_0$ is same shape as $\mathbf{x}_1$
3. Sample time: $t \sim \mathcal{U}[0,1]$
4. Build an intermediate state: $\mathbf{x}_t = (1-t)\mathbf{x}_0 + t\mathbf{x}_1$
5. Compute loss: $L = \|\mathbf{v}_\theta(\mathbf{x}_t, t) - (\mathbf{x}_1 - \mathbf{x}_0)\|^2$ \& backpropagate

## Example 1

Consider a $2 \times 2$ image

$$\mathbf{x}_1 = \begin{bmatrix} 0.8 & 0.2 \\ 0.1 & 0.9 \end{bmatrix}$$

Let $\mathbf{x}_0$ be sampled noise

$$\mathbf{x}_0 = \begin{bmatrix} -0.4 & 1.2 \\ 0.3 & 0.7 \end{bmatrix}$$

Let $t = 0.5$. Then we build $\mathbf{x}_t$

$$\mathbf{x}_{0.5} = (1-0.5)\begin{bmatrix} -0.4 & 1.2 \\ 0.3 & 0.7 \end{bmatrix} + 0.5\begin{bmatrix} 0.8 & 0.2 \\ 0.1 & 0.9 \end{bmatrix}$$

$$\mathbf{x}_{0.5} = \begin{bmatrix} 0.2 & 0.7 \\ 0.2 & 0.1 \end{bmatrix}$$

Compute target velocity $(\mathbf{x}_1 - \mathbf{x}_0)$

$$\mathbf{x}_1 - \mathbf{x}_0 = \begin{bmatrix} 0.8 - (-0.4) & 0.2 - 1.2 \\ 0.1 - 0.3 & 0.9 - 0.7 \end{bmatrix} = \begin{bmatrix} 1.2 & -1.0 \\ -0.2 & 1.6 \end{bmatrix} = \mathbf{v}_{tgt}$$

Suppose our model predicts

$$\mathbf{v}_\theta(\mathbf{x}_t, t) = \mathbf{v}_\theta(\mathbf{x}_{0.5}, 0.5) = \begin{bmatrix} 1.1 & -0.9 \\ -0.3 & 1.5 \end{bmatrix} = \mathbf{v}_{pred}$$

Compute residual

$$\mathbf{v}_\theta(\mathbf{x}_{0.5}, 0.5) - (\mathbf{x}_1 - \mathbf{x}_0) = \begin{bmatrix} -0.1 & +0.1 \\ -0.1 & -0.1 \end{bmatrix} = \mathbf{r}$$