<h1 align="center">Diffusion Maps</h1>
<div align="center"><i>A method for analyzing and organizing high dimensional, noisy, and unordered data.</i></div>
<br>
This project reveals the diffusion map and demonstrates 3 different ways to construct this technique.

## Contents
- [Introduction to Diffusion Maps](#Introduction-to-Diffusion-Maps)
- [Constructions](#Constructions)
- [Demonstrations](#Demonstrations)
- [Conclusion](#Conclusion)
- [References](#References)


## Introduction to Diffusion Maps
Diffusion maps reduced the dimension of points by finding a lower-dimensional manifold in which points are embedded.

### :question: *Why should we used diffusion maps?*
- allow data in data space to have non-linear shape
- robust to noise perturbation 
- computationally inexpensive

  
### :question: *How should we construct diffusion maps?*

1. ***Construct the affinity (kernel) matrix K*** 
    - The entry of K is small if two data points are far away from each other in the data space, and is large if opposite.


2. ***Construct the diffusion matrix P***
    - P preserves the connectivity between data points.  
    - The entry of P is consider as the probability of jumping from one data point to another in one step of a random walk.
    
    Note that one can stablize the random walk by taking powers of P, this is called the diffusion process. This is because through the *diffusion process*, the probability of following a path along the underlying geometric structure of the data set increases. (Since data are dense and highly connected along the geometric structure, and pathways form along short and high probability jumps.)


3. ***Construct the diffusion map Y***
    - Y maps corrdinates of data between kernel space and diffusion space.  
    - Dimensional reduction is done by neglecting certain dimensions in the diffusion space. This is because the value of eigenvalues indicate the importance of each dimension, and the left eigenvectors of P form a basis of the diffusion space.



## Constructions
There is no unique way to define the related matrices, that is why researchers are allowed to construct the diffusion map in thier own way.  
I will implement some constructions in this section.

:pencil2: Construction 1, `Diff_Map_AnnLeeMethod.ipynb`, is based on [Ann Lee's Matlab code](https://reurl.cc/E3Ykv), which is my first sight to diffusion maps. 

:pencil2: Construction 2, `Diff_Map_ManorMethod.ipynb`, improves construction 1 by picking the scaling parameter *sigma* automatically, moreover, selecting the local scaling parameter *sigma_i* for each data point $x_i$ instead of selecting a single *sigma*.  
The selection of *sigma_i*, which can be done by studying the local statistics of the neighborhood of *x_i*, allows self-tuning of the point-to-point distances. A simple choice is the square of the distance between *x_i* and its *s*'th nearest neighbor. By experimenting, *s*=7 gives good results for all data.*

:pencil2: Construction 3, `Diff_Map.ipynb`, constructs the diffusion map in my own way, which combined the knowledge in [1], [2], and [3].




### :pencil2: Construction 1, `Diff_Map_AnnLeeMethod.ipynb` 

Given 
- <img src="https://latex.codecogs.com/gif.latex?X=\{x_1,...,x_n\}\in\mathbb{R}^p "/>, the data set,
- *sigma*, the scaling parameter, 
- *c*, the number of the eigenvalues we computed, 
- *clusters*, the number of groups of data,

Ann Lee constructed the diffusion map by using the following steps:

1. ***Construct the affinity matrix K***  
Define K with entries 
<img src="https://latex.codecogs.com/png.latex?K_{ij}=\exp(-\frac{d(x_i,x_j)^2}{4\sigma})." />

2. ***Construct the normalize affinity matrix Q***  
Define Q with entries 
<img src="https://latex.codecogs.com/gif.latex?\Q_{ij}=\frac{K_{ij}}{\sqrt{\sum_{j=1}^nK_{ij}\sum_{i=1}^nK_{ij}}}. "/>
Note that the multiplication and division are elementwise.


3. ***Calculate the first $c$ largest eigenvalues and eigenvectors of the diffusion matrix P***  
The eigenvalues of $P$ is equal to the eigenvalues of Q.  
The right and left eigenvectors of P are 
<img src="https://latex.codecogs.com/png.latex?\psi_j=e_je_1,\quad\phi_j=e_j/e_1" />
respectively, where *e_j* is the eigenvector of the corresponding j'th eigenvalue of Q.  
Note that the multiplication and division are elementwise.


4. ***Define a diffusion map Y***  
Define Y with columns
<img src="https://latex.codecogs.com/png.latex?Y_j=[\frac{\lambda_j}{1-\lambda_j}\psi_j]." />


5. ***Cluster via k-means***  
Using k-means to cluster data into $clusters$ groups in the diffusion space.



### :pencil2: Construction 2, `Diff_Map_ManorMethod.ipynb` 

Given 
- <img src="https://latex.codecogs.com/gif.latex?X=\{x_1,...,x_n\}\in\mathbb{R}^p "/>, the data set,
- *sigma_i*, the local scaling parameter, 
- *c*, the number of the eigenvalues we computed, 
- *clusters*, the number of groups of data,

I constructed the diffusion map refered to [3]:

1. ***Construct the affinity matrix K***  
Define K with entries 
<img src="https://latex.codecogs.com/png.latex?K_{ij}=\exp(-\frac{d(x_i,x_j)^2}{\sigma_i\sigma_j})." />
where *sigma_i* is the square of the distance between *x_i* and its *s*'th nearest neighbor.


2. ***Construct the normalize affinity matrix Q***  
Define a *n*n* diagonal matrix D with entries 
<img src="https://latex.codecogs.com/png.latex?D_{ii}=(\sum_{j=1}^nK_{ij})^{1/2}." />
Constructs Q by  
<img src="https://latex.codecogs.com/png.latex?Q:=D^{-1}KD^{-1}." />


3. ***Calculate the eigenvectors of Q***  


4. ***Define a diffusion map***  
Define a diffusion map Y with entries
<img src="https://latex.codecogs.com/png.latex?Y_{ij}=\frac{Z_{ij}}{\sqrt{\sum_{j=1}^{c}Z_{ij}^2}}," />
where Z is the unitary matrix having the corresponding eigenvectors as columns.  
Note that the multiplication and division are elementwise.


5. ***Cluster via k-means***  
Using k-means to cluster data into $clusters$ groups in the diffusion space.



### :pencil2: Construction 3, `Diff_Map.ipynb`

Given 
- $X=\{x_1,...,x_n\} \in \mathbb{R}^p$, the data set,
- $s$, the local scaling parameter, 
- $c$, the number of the eigenvalues we computed, 
- $clusters$, the number of groups of data,

I constructed the diffusion map in the following steps:

1. ***Construct the affinity matrix***  
Define a Guassian kernel matrix $K$ with entries 
$$ K_{ij} = \exp(-\frac{d(x_i,x_j)^2}{\sigma_i \sigma_j}), $$
where $ \sigma_i=d(x_i,x_s)^2 $ and $x_s$ is the $s$'th neighbor of point $x_i$.


2. ***Construct the normalize affinity matrix $Q$***  
Define a $n*n$ diagonal matrix $D$ with entries 
$$ D_{ii}=(\sum_{j=1}^n K_{ij})^{1/2}, $$
constructs the normalize affinity matrix $$ Q:=D^{-1}KD^{-1}. $$


3. ***Calculate the first $c$ largest eigenvalues and the corresponding eigenvectors of the diffusion matrix $P$***  
The eigenvalues of $P$ is equal to the eigenvalues of $Q$.  
The right and left eigenvectors of $P$ are $$ \psi_k=D^{-1} e_j ,\quad \phi_k=D e_j $$ respectively, where $e_j$ is the eigenvector of the corresponding j-th eigenvalue of $Q$.


4. ***Define a diffusion map*** 
Define a diffusion map $Y$ with columns $$ Y_j=[\lambda_j \psi_j]. $$


5. ***Cluster via k-means***  
Using k-means to cluster data into $clusters$ groups in the diffusion space.



## Demonstrations

All algorithms mention above can effectively cluster data. I will demonstrate some results in this section. 

imageimage




## Conclusions
This project described the full view of diffusion map, a method of non-linear dimensionality reduction. It showed how to use a kernel function to recover the diffusion matrix and distance, and the relationship between diffusion distance and map. It showed that in the lower-dimensional space, the structure of data still be preserved and data can be analyzed more easily.

## References
[1] R.R. Coifman and S. Lafon, Diffusion maps, Applied and computational harmonic analysis, 21(1):5–30, 2006  
[2] J. de la Porte, B. M. Herbst, W. Hereman and S. J. van der Walt., An Introduction to Diffusion Maps, Proceedings of the Nineteenth Annual Symposium of the Pattern Recognition Association of South Africa, 2008  
[3] L. Zelnik-Manor and P. Perona, Self-Tuning Spectral Clustering, Advances in Neural Information Processing Systems 17, pp. 1601-1608, 2005, (NIPS’2004)


```python

```
