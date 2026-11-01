# Mathematical Analysis of the LSTM Component

## A. LSTM Equations

Both LSTM layers of the model (encoder and decoder) use the standard LSTM cell (1) with a forget gate (2).

With \(h_0 = 0\) and \(c_0 = 0\), at each time step \(t\):

\[
i_t = \sigma(W_i x_t + U_i h_{t-1} + b_i)
\]

\[
f_t = \sigma(W_f x_t + U_f h_{t-1} + b_f)
\]

\[
g_t = \tanh(W_g x_t + U_g h_{t-1} + b_g)
\]

\[
o_t = \sigma(W_o x_t + U_o h_{t-1} + b_o)
\]

\[
c_t = f_t \odot c_{t-1} + i_t \odot g_t
\]

\[
h_t = o_t \odot \tanh(c_t)
\]

Where:

- \(\sigma\) is the sigmoid function.
- \(\tanh\) is the hyperbolic tangent function.
- \(\odot\) denotes element-wise multiplication.

## B. Property and Mathematical Proof

We prove that the cell state remains bounded. Specifically, for \(c_0 = 0\), every entry satisfies:

\[
|c_t| \leq t
\]

### Gate Properties

The forget gate \(f_t\) and input gate \(i_t\) are sigmoid outputs, so:

\[
0 < f_t < 1, \qquad 0 < i_t < 1
\]

The candidate cell state \(g_t\) is a tanh output, so:

\[
-1 < g_t < 1
\]

Therefore:

\[
|g_t| < 1
\]

### Base Case

For \(t = 0\):

\[
c_0 = 0
\]

Thus:

\[
|c_0| = 0 \leq 0
\]

The property holds for the base case.

### Inductive Hypothesis

Assume that for some time step \(t\), the following property holds:

\[
|c_t| \leq t
\]

### Inductive Step

We need to prove that:

\[
|c_{t+1}| \leq t+1
\]

Using the LSTM cell-state equation:

\[
c_{t+1} = f_{t+1} \odot c_t + i_{t+1} \odot g_{t+1}
\]

Applying the triangle inequality:

\[
|c_{t+1}|
\leq f_{t+1}|c_t| + i_{t+1}|g_{t+1}|
\]

Using the gate bounds and the inductive hypothesis:

\[
|c_{t+1}|
\leq 1 \cdot t + 1 \cdot 1
\]

Therefore:

\[
|c_{t+1}| \leq t+1
\]

The property holds for the next time step.

### Conclusion

By mathematical induction:

\[
\boxed{|c_t| \leq t \quad \forall t \geq 0}
\]

For our 16-frame temporal windows:

\[
\boxed{|c_t| \leq 16}
\]

The proof does not depend on the weights or the input values. Therefore, it holds for both LSTM layers, provided their initial cell states are zero.

## C. Empirical Verification

We computed the encoder LSTM cell states for both the Scene A model and the Scene B model using 236 windows of 16 consecutive frames with a spatial resolution of \(112 \times 112\), sampled from 25 AURA videos.

Up to 10 windows were selected from the beginning of each video.

Our independent computation of the LSTM equations matches the models' encoder outputs, with the following maximum absolute differences:

| Model | Maximum Absolute Difference |
|---|---:|
| Scene A | \(4.0 \times 10^{-7}\) |
| Scene B | \(5.7 \times 10^{-7}\) |

The theoretical bound holds at all 16 time steps for both models.

At \(t = 16\), the maximum observed cell-state values are:

| Model | Maximum Observed Value | Percentage of Theoretical Bound |
|---|---:|---:|
| Scene A | 7.25 | 45% |
| Scene B | 8.22 | 51% |

These observations are consistent with the theoretical bound of 16.

## D. Limitations

The empirical verification uses a sample of windows taken from the beginning of each video, with consecutive frames in both scenes.

The empirical check illustrates the proven mathematical property but does not replace the mathematical induction proof.

Furthermore, the empirical verification described above was performed on the encoder LSTM. The mathematical proof applies to the decoder as well, provided that its initial cell state is zero.
