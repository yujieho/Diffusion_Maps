<h1 align="center">Diffusion Maps</h1>
<div align="center"><i>A method for analyzing and organizing high dimensional, noisy, and unordered data.</i></div>
<br>

This project introduces the diffusion map and demonstrates 3 different ways to construct this technique.

## Contents
- [Introduction](#Introduction)
- [Implementations](#Implementations)
- [Demonstrations](#Demonstrations)
- [References](#References)


## Introduction
Diffusion maps reveals data structures by finding a lower-dimensional manifold in which points are embedded.
<p align='center'><img src="results/intro.png" alt="intro" height="150" /></p>


### :mag: *Why should we use diffusion maps?*
- allow data in data space to have non-linear shape
- robust to noise perturbation 
- computationally inexpensive

  
### :mag: *How diffusion maps work?*


***Diffusion matrix P***
- By using the kernel matrix, P preserves the connectivity between data.  
- The entry of P is consider as the probability of jumping from one data point to another in one step of a random walk.  

***Diffusion map Y***
- By using certain eigenvectors and eigenvalues of P, Y maps data from data space to a lower-dimensional diffusion space.  
- Data that are related in the data space will gather in the diffusion space, forming clusters.  
- *Dimension reduction*: Neglecting certain eigenvalues and their eigenvectors.  
    (Since the eigenvalue indivates the importance of its eigenvectors.)  

***Diffusion process***
- This process stabilize the random walk by deriving the power of P to *t*.
- *Stabilization*: Increasing *t* increases the probability of following a path along the underlying geometric structure of data.  
    (Since data are dense and highly connected along the geometric structure, and pathways form along short and high probability jumps.)



## Implementations
There are various way to construct a diffusion map, here are the introductions of three different algorithms I have done.

:round_pushpin: Construction 1, `DM-AnnLeeMethod.ipynb`, is my first sight to diffusion maps.  
- Based on [Ann Lee's Matlab code](https://reurl.cc/E3Ykv).  
- Allows one to increase the time parameter *t* in the diffusion process.  
(Since my data are not that noisy, I found the process is not necessary for them to have good results, and omitted it in subsequent constructions.) 

:round_pushpin: Construction 2, `DM-ManorMethod.ipynb`, improves construction 1.
- Based on the paper [3]. 
- Improves construction 1 by picking a manually selected parameter automatically.

:round_pushpin: Construction 3, `DM.ipynb`, combines knowledges I learned.
- The diffusion map that constructed in my own way. 



## Demonstrations
All algorithms I have done can effectively cluster data.  

I will demonstrate some results in this section. 



:pencil2: `Data.mat`, number of data: 2000.

<p align='center'>
    <img src="results/1-dataspace.png" height="220" />
    <img src="results/1-diffusionspace.png" height="200"/>
    <img src="results/1-clustering.png" height="220"/>
</p>


:pencil2: `Data2.mat`, number of data: 299.

<p align='center'>
    <img src="results/2-dataspace.png" height="220" />
    <img src="results/2-diffusionspace.png" height="200"/>
    <img src="results/2-clustering.png" height="220"/>
</p>


:pencil2: `Data3.mat`, number of data: 303.

<p align='center'>
    <img src="results/3-dataspace.png" height="220" />
    <img src="results/3-diffusionspace.png" height="200"/>
    <img src="results/3-clustering.png" height="220"/>
</p>


:pencil2: `Data5.mat`, number of data: 622.

<p align='center'>
    <img src="results/5-dataspace.png" height="220" />
    <img src="results/5-diffusionspace.png" height="200"/>
    <img src="results/5-clustering.png" height="220"/>
</p>


See more results in `results`.



## References
[1] R.R. Coifman and S. Lafon, Diffusion maps, Applied and computational harmonic analysis, 21(1):5–30, 2006  
[2] J. de la Porte, B. M. Herbst, W. Hereman and S. J. van der Walt., An Introduction to Diffusion Maps, Proceedings of the Nineteenth Annual Symposium of the Pattern Recognition Association of South Africa, 2008  
[3] L. Zelnik-Manor and P. Perona, Self-Tuning Spectral Clustering, Advances in Neural Information Processing Systems 17, pp. 1601-1608, 2005, (NIPS’2004)


