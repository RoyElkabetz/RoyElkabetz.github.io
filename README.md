# Welcome
![](Figures/llama_Euler.jpg)

So, why llama you ask?
well, I'm not really sure, I think that llamas are the new unicorns and unicorns are obviously cool
so this thing is actually writing itself as you can see.

---

[Tensor Networks and Probabilistic Graphical Models Duality](#tensor-networks-and-probabilistic-graphical-models-duality) 


# Tensor Networks and Probabilistic Graphical Models Duality

In the [papper](https://arxiv.org/abs/1710.01437) of Elina Robeva (MIT) and Anna Seigal (Berkeley) "*Duality of Graphical Models and Tensor Networks*", following proposition (3.7) which states that marginalization in graphical models and contractions in tensor networks (TN) are actually the same thing and can be described using the next equation

![](https://latex.codecogs.com/gif.latex?P%28x_W%29%3D%5Csum_%7Bx_u%5Cin%20%5Cleft%20%5B%20n_u%20%5Cright%20%5D%3A%20u%5Cnotin%20W%7D%20%5Cprod_%7BC%5Cin%20%5Cmathcal%7BC%7D%7D%20%5Cleft%20%28T_C%20%5Cright%20%29_%7B%5Cleft%20%5C%7Bx_C%3Au%5CinC%20%5Cright%20%5C%7D%7D)

where we get the marginal on the distribution over the ![](https://latex.codecogs.com/gif.latex?x_W) veriables as a sum of all other veriables (or tensor entries in TN description) over the product of all tensors in the tensor network.

All the work done in the papper above is in the context of Markov Random Fields (MRF) graphical models, and what I would like to do next is to workout the duality notion between TN and [Factor Graphs](https://en.wikipedia.org/wiki/Factor_graph) (FG) which is another form of graphical models and between TN and Double-Edge Normal Factor Graphs (DEnFG) which is a form of a Normal Factor Graph. Let's work out the [MPS](https://en.wikipedia.org/wiki/Matrix_product_state) TN although the conclusions would be true for any general TN.
 
![](Figures/MPS_factor_graph-1.png)

In the figure above it is possible to see how the MPS TN transforms to a FG under the duality, where every edge (bond) in the TN become a node (pink circle) with alphabet size same as the edge (bond) dimension. The ![](https://latex.codecogs.com/gif.latex?x_i) marginalization over the FG probability distribution is given by

![](https://latex.codecogs.com/gif.latex?P%5Cleft%20%28%20x_i%20%5Cright%20%29%3D%5Cfrac%7B1%7D%7BZ%7D%5Csum_%7B%5Cleft%20%5C%7B%20x_j%3Aj%5Cneq%20i%20%5Cright%20%5C%7D%7D%5Cprod_%7Bf%5Cin%5Cmathcal%7BF%7D%7Df%5Cleft%20%28%20x_%7B%5Cpartial%20f%7D%20%5Cright%20%29)

where every factor is identical to its dual tensor ![](https://latex.codecogs.com/gif.latex?f%5Cleft%20%28%20x_%7B%5Cpartial%20f%7D%20%5Cright%20%29%3DT_C) such that ![](https://latex.codecogs.com/gif.latex?x_%7B%5Cpartial%20f%7D%20%3Dx_C), so contraction and marginalization are the same also here. The only difference in the FG case is that the tansors and factors are identical where in the MRF case the edges of the TN become nodes and the tensors become *hyperedges*.

## How can we utilize the duality for computations
### TN Contraction
In general, given a TN, a common operation would be to calculate the expectation value of some local observable i.e the local operator ![](https://latex.codecogs.com/gif.latex?%5Cmathcal%7BO%7D_i) which is operating over the ![](https://latex.codecogs.com/gif.latex?i%5E%7Bth%7D) spin. In the 7 spins MPS TN case, the local observeble expectation over the first spin would look like that

![](Figures/mps_expectetion-1.png)

To compute the expectation above one would need to contract the whole network. If for example the network maximal bond dimension is ![](https://latex.codecogs.com/gif.latex?D) and the spins dimension is ![](https://latex.codecogs.com/gif.latex?p) the contraction time would be ![](https://latex.codecogs.com/gif.latex?O%5Cleft%20%28%20pD%5E4N%20%5Cright%20%29) where in the case above ![](https://latex.codecogs.com/gif.latex?N=7).

### Loopy Belief Propagation ([LBP](https://en.wikipedia.org/wiki/Belief_propagation))
Another way to calculate this expectation is to transform the TN to a FG and then implement LBP which is a message-passing algorithm used for inference. For each time step we would make a full update over all the messages from nodes to factros and vice versa. The messages (up to normalization factors) are given by 

![](https://latex.codecogs.com/gif.latex?m_%7Bi%5Crightarrow%20a%7D%5E%7Bt&plus;1%7D%5Cleft%28x_%7Bi%7D%5Cright%29%26%5Cpropto%5Cprod_%7Bb%5Cin%20N%5Cleft%28i%5Cright%29/a%7Dm_%7Bb%5Crightarrow%20i%7D%5E%7Bt%7D%5Cleft%28x_%7Bi%7D%5Cright%29%5C%5Cm_%7Ba%5Crightarrow%20i%7D%5E%7Bt&plus;1%7D%5Cleft%28x_%7Bi%7D%5Cright%29%26%5Cpropto%5Csum_%7B%5Cmathbf%7Bx%7D_%7Ba%7D/x_%7Bi%7D%7Df%5Cleft%28%5Cmathbf%7Bx%7D_%7Ba%7D%5Cright%29%5Cprod_%7Bj%5Cin%20N%5Cleft%28a%5Cright%29/i%7Dm_%7Bj%5Crightarrow%20a%7D%5E%7Bt%7D%5Cleft%28x_%7Bj%7D%5Cright%29)

where the ![](https://latex.codecogs.com/gif.latex?i)'s are the node indices and the ![](https://latex.codecogs.com/gif.latex?a)'s are the factor indices. In the case of tree FG there is a theorem by [Yedidia et al](http://www.merl.com/publications/docs/TR2001-22.pdf) which states that the Belief Propagation (no loops) messages converge to their *true value*. Later using the messages we can calculate the node and factor beliefs (marginals) via

![](https://latex.codecogs.com/gif.latex?b%5Cleft%28x_%7Bi%7D%5Cright%29%26%5Cpropto%5Cprod_%7Ba%5Cin%5Cpartial%20i%7Dm_%7Ba%5Crightarrow%20i%7D%5Cleft%28x_%7Bi%7D%5Cright%29%5C%5Cb%5Cleft%28x_%7B%5Cpartial%20a%7D%5Cright%29%26%5Cpropto%20f%5Cleft%28x_%7B%5Cpartial%20a%7D%5Cright%29%5Cprod_%7Bi%5Cin%5Cpartial%20a%7Dm_%7Bi%5Crightarrow%20a%7D%5Cleft%28x_%7Bi%7D%5Cright%29)

If the FG has loops the LPB might not even converge and if it does there is no guarantee that the messages will converge to their true value. So, for loopy FG the beliefs are approximations to the true FG marginals. This is called the Bethe approximation which approximate the graph as if it was a tree. The quality of the approximation would depend on the number and size of loops in the graph.

If the LBP does converge, it would take ![](https://latex.codecogs.com/gif.latex?O%5Cleft%20%28%20tNdD%5Ed%20%5Cright%20%29) where ![](https://latex.codecogs.com/gif.latex?t) is the number of full update iterations, ![](https://latex.codecogs.com/gif.latex?N) is the number of spins, ![](https://latex.codecogs.com/gif.latex?d) is the maximal number of neighbors (for all factors) and ![](https://latex.codecogs.com/gif.latex?D) is the maximal node alphabet. 

All this is working very well in general, the problem arises when our tensors in the TN have complex entries. The complex value entries objective is to describe the phases in the mixed state wavefunction. In that case the product of all the factros in the graph would be a complex valued tensor so it can't be interpreted as a probability distribution. This means that we need to find other graphical model formalism which can support probability interpretation of complex tensors.

## Is there a way to go around the complex valued Tensors
If you recall, in quantum information theory the techniques to calculate an expectation value of an operator ![](https://latex.codecogs.com/gif.latex?%5Cmathcal%7BO%7D) given a wave function ![](https://latex.codecogs.com/gif.latex?%5Cleft%7C%5Cpsi%5Cright%5Crangle) is through 

![](https://latex.codecogs.com/gif.latex?%5Cleft%5Clangle%20%5Cmathcal%7BO%7D%5Cright%5Crangle%20_%7B%5Cpsi%7D%3D%5Cleft%5Clangle%20%5Cpsi%5Cleft%7C%5Cmathcal%7BO%7D%5Cright%7C%5Cpsi%5Cright%5Crangle%20%3D%5Ctext%7BTr%7D%5Cleft%5B%5Cleft%5Clangle%20%5Cpsi%5Cleft%7C%5Cmathcal%7BO%7D%5Cright%7C%5Cpsi%5Cright%5Crangle%20%5Cright%5D%3D%5Ctext%7BTr%7D%5Cleft%5B%5Cleft%7C%5Cpsi%5Cright%5Crangle%20%5Cleft%5Clangle%20%5Cpsi%5Cright%7C%5Cmathcal%7BO%7D%5Cright%5D%3D%5Ctext%7BTr%7D%5Cleft%5B%5Crho%5Cmathcal%7BO%7D%5Cright%5D)

where ![](https://latex.codecogs.com/gif.latex?%5Crho%3D%5Cleft%7C%5Cpsi%5Cright%5Crangle%20%5Cleft%5Clangle%20%5Cpsi%5Cright%7C) is the density matrix. The density matrix is a [psd matrix](https://en.wikipedia.org/wiki/Definiteness_of_a_matrix#Definitions_for_complex_matrices) whixh means its eigenvalues are all larger or equal to zero. The purpose of all this is to show that the usage of density matrices instead of wavefunctions is much more efficient in the graphical models world. Although in the TN world it actually does not matter, i.e the MPS density matrix would look like

![](Figures/MPS_rho.jpg)

where I put deferent colors just to illustrate batter the complex conjugate of the wavefunction TN which is in the upper side of the figure. Now, calculating the expectation of a local observable ![](https://latex.codecogs.com/gif.latex?%5Cmathcal%7BO%7D_1) would mean to contract the same network as in the figure above of MPS expectation. On the other hand, we could use the [DEnFG](##Double-Edge-normal-Factor-Graphs) formalism to calculate the same expectation (even for complex tensors) in the graphical models world.

## Double-Edge normal Factor Graphs (DEnFG)
The first time I was encountered with DEnFG was in the [papper](https://ieeexplore.ieee.org/abstract/document/8277985) of Michael X. Cao and Pascal O. Vontobel. Althogh to me it looks like their work has some gabs in the Bethe approximation reformulation, the method principle of handeling complex valued tensors is working well.




