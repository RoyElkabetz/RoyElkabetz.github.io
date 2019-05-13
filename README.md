## Welcome
![](llama.jpg)

So, why llama you ask?
well, I'm not really sure, I think that llama is the new pineapple and pineapples are obviously cool
so this thing is actually writing itself as you can see.

---

[Tensor Networks and Probabilistic Graphical Models Duality](#tensor-networks-and-probabilistic-graphical-models-duality) 


# Tensor Networks and Probabilistic Graphical Models Duality

In the [papper](https://arxiv.org/abs/1710.01437) of Elina Robeva (MIT) and Anna Seigal (Berkeley) "*Duality of Graphical Models and Tensor Networks*", following proposition (3.7) which states that marginalization in graphical models and contractions in tensor networks (TN) are actually the same thing and can be described using the next equation

![](https://latex.codecogs.com/gif.latex?P%28x_W%29%3D%5Csum_%7Bx_u%5Cin%20%5Cleft%20%5B%20n_u%20%5Cright%20%5D%3A%20u%5Cnotin%20W%7D%20%5Cprod_%7BC%5Cin%20%5Cmathcal%7BC%7D%7D%20%5Cleft%20%28T_C%20%5Cright%20%29_%7B%5Cleft%20%5C%7Bx_C%3Au%5CinC%20%5Cright%20%5C%7D%7D)

where we get the marginal on the distribution over the ![](https://latex.codecogs.com/gif.latex?x_W) veriables as a sum of all other veriables (or tensor entries in TN description) over the product of all tensors in the tensor network.

All the work done in the papper above is in the context of Markov Random Fields (MRF) graphical models, and what I would like to do next is to workout the duality notion between TN and [Factor Graphs](https://en.wikipedia.org/wiki/Factor_graph) (FG) which is another form of graphical models and between TN and Double-Edge Normal Factor Graphs (DEnFG) which is a form of a Normal Factor Graph. Let's work out the [MPS](https://en.wikipedia.org/wiki/Matrix_product_state) TN although the conclusions would be true for any general TN.

Given a open boundary condition MPS as illustrated in the figure below
![]()







