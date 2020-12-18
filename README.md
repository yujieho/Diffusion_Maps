# Diffusion_Maps

In this project, I will introduce the diffusion Maps, a non-linear dimensionality reduction method, and show three different ways to construct this technique.

To take a quick full view of diffusion maps, one can read the Diff_Map.ipynb , in which has a complete discription of this method, including the definition of nomenclatures.
For me, I learn this technique starting from converting Ann Lee's matlab code into Python, and show the result in Diff_Map_AnnLeeMethod.ipynb . To understand the meaning of the matlab code, I read papers that discribe this method, and found out that there's a paper provided a better version of constructing diffusion maps, in which we can choose an request parameter, which often picked manually and can only be guessed, automatically. The introduction and implementation of this method is discribe in Diff_Map_ManorMethod.ipynb . Finally I consolidate papers I have studied and make a full discription of diffusion maps in Diff_Map.ipynb .

One can compare these three codes, it is not hard to find that there is no unique way to construct a map, some method seems very different, but still can get the result as good as another.

After reading these three codes, one should learn the steps of constructing a diffusion map.
