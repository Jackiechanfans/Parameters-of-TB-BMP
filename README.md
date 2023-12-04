# Parameters of TB-BMP
Parameters of the threshold-based binary message passing (TB-BMP) decoder with memory for product codes (PCs) with different component BCH codes are now available!

## What is TB-BMP
The TB-BMP is a modification of [iBDD-SR](https://ieeexplore.ieee.org/abstract/document/8832202) by introducing a **memory unit** and a **threshold**. In the proposed decoding algorithm, the soft reliability of the bounded distance decoding (BDD) output at the current half-iteration is a weighted sum of the BDD output, the channel reliability, and the content of the memory unit, where the content of the memory unit at the current half-iteration is related to the selected threshold and the BDD output at last half-iteration.

<p align="center">
<img width="1000px" src="figures/TB_BMP.png">
</p>

The above figure shows the block diagram of TB-BMP at half-iteration $\ell$. Here, $w^{(\ell)}$ is a scaling factor which can be obtained via density evolution (DE) of TB-BMP, $L$ is the value of log-likehood ratio (LLR), and the soft reliability $u^{(\ell)}$ is given as $$u^{(\ell)} = w^{(\ell)} \bar{u}^{(\ell)} + \beta^{(\ell-1)}w^{(\ell)}B^{(\ell-1)} + L.$$ 

### TB-BMP-1
The value of $B^{(\ell)}$ is a function of the value of $u^{(\ell)}$, the BDD output $\bar{u}^{(\ell)}$, and the flipping threshold $T$. 

- If the absolute value of the soft reliability ${u}^{(\ell)}$ is less than the threshold $T$ or the decoding declares a failure in the $\ell$-th  half-iteration, we have $B^{(\ell)}=-\mathrm{sgn}(u^{(\ell)})$; 
- otherwise, the output of the BDD is saved to the memory unit and we have $B^{(\ell)}=\bar{u}^{(\ell)}$.

This TB-BMP decoder is referred to as `TB-BMP-1`.


### TB-BMP-2
The value of $B^{(\ell)}$ is the value of $u^{(\ell)}$ and the flipping threshold $T$.

- If the absolute value of the soft reliability ${u}^{(\ell)}$ is less than the threshold $T$, we have $B^{(\ell)}=-\mathrm{sgn}(u^{(\ell)})$; 
- otherwise, the output of the BDD is saved to the memory unit and we have $B^{(\ell)}=\mathrm{sgn}(u^{(\ell)})$.

This TB-BMP decoder is referred to as `TB-BMP-2`.

## Parameters $w^{(\ell)}$ and $\beta^{(\ell)}$
The values of parameters $w^{(\ell)}$ and $\beta^{(\ell)}$ for both TB-BMP-1 and TB-BMP-2 are obtained based on `the DE of TB-BMP-2 with extrinsic BDD`. In the following, extrinsic message passing and intrinsic message passing are abbreviated as EMP and IMP, respectively.

### The calculation of $w^{(\ell)}$ 
Let $x^{(\ell)}$ denotes the average error probability of the TB-BMP-2 output at half-iteration $\ell$. The scaling factor $w^{(\ell)}$ based on the analytical results of the DE is given as
$$w^{(\ell)} = \log \Bigl(\frac{ f^c(x^{(\ell-1)},p_{\mathrm{ch}}) }{ f^e(x^{(\ell-1)},p_{\mathrm{ch}}) } \Bigr),$$
where
$$f^c(x,p_{\mathrm{ch}}) = \sum_{j=0}^{n-1} \binom{n-1}{j} (x)^{j} (1-x)^{n-j-1} \times \Bigl( p_{\mathrm{ch}}P^c(j) + (1-p_{\mathrm{ch}})Q^c(j) \Bigr) $$
and
$$f^e(x,p_{\mathrm{ch}}) = \sum_{j=0}^{n-1} \binom{n-1}{j} (x)^{j} (1-x)^{n-j-1} \times \Bigl( p_{\mathrm{ch}}P^e(j) + (1-p_{\mathrm{ch}})Q^e(j) \Bigr).$$
Here, $P_{\mathrm{ch}}$ is the error probability of the channel ouput and $P^c(j)$, $Q^c(j)$, $P^e(j)$, and $Q^e(j)$ can be found in the [paper](https://ieeexplore.ieee.org/abstract/document/8832202) of iBDD-SR.

### The calculation of $\beta^{(\ell)}$
We impose the constraint that $0 \leq \beta^{(\ell)} < 1$. Particularly, for a fixed $T$, the parameter $\beta^{(\ell)}$ is given as
$$\beta^{(\ell)} = \frac{P( -T < u^{(\ell)} < 0 )}{ P( u^{(\ell)} < T ) }.$$
The values of $\beta^{(\ell)}$ are obtained during the process of DE.

### Parameters of $w^{(\ell)}$ and $\beta^{(\ell)}$ for different PCs
In the following simulations, the parameters $w^{(\ell)}$ and $\beta^{(\ell)}$ for both TB-BMP-1 and TB-BMP-2 are selected via the DE of TB-BMP-2. Furthermore, both TB-BMP-1 and TB-BMP-2 are executed for the first 10 iterations and iBDD are then executed for the last 2 iterations. These parameters are obtained by setting the target BER$=10^{-7}$ after 20 half-iterations for DE.

For IMP, the DE analysis provide a guideline to select parameters $w^{(\ell)}$ and $\beta^{(\ell)}$ according to the optimal $T_{\mathrm{IMP}}$ obtained via Monte-Carlo simulations. We only consider performances of IMP-based TB-BMP-1 with the optimal $T_{\mathrm{opt}}^{(1)}$ and IMP-based TB-BMP-2 with the optimal $T_{\mathrm{opt}}^{(2)}$, where parameters $w^{(\ell)}$ and $\beta^{(\ell)}$ are selected according to $T_{\mathrm{opt}}^{(1)}$ and $T_{\mathrm{opt}}^{(2)}$.

#### The PC with (255,239,2) BCH code
We first present the below figure the simulation results of both TB-BMP-1 and TB-BMP-2 with $T_{\mathrm{opt}}^{(1)} = T_{\mathrm{opt}}^{(2)} = 2.8$ for the PC based on (255,239,2) BCH code. 
<p align="center">
<img width="600px" src="figures/sim_255.png">
</p>

For both $T_{\mathrm{opt}}^{(1)} = T_{\mathrm{opt}}^{(2)} = 2.8$, we have
- $w^{(0 \to 19)}=$ 3.10, 3.16, 3.54, 3.60, 3.69, 3.75, 3.80, 3.85, 3.89, 3.94, 4.00, 4.06, 4.14, 4.25, 4.41, 4.67, 5.14, 6.14, 8.27, 12.19
