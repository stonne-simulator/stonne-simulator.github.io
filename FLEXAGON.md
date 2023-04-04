# Flexagon

Sparsity is a growing trend in modern DNN models. Existing Sparse-Sparse Matrix Multiplication (SpMSpM) accelerators are tailored to a particular SpMSpM dataflow (i.e., Inner Product, Outer Product or Gustavsons), that determines their overall efficiency. We demonstrate that this static decision inherently results in a suboptimal dynamic solution. This is because different SpMSpM kernels show varying features (i.e., dimensions, sparsity pattern, sparsity degree), which makes each dataflow better suited to different data sets.

<p align="center">
 <img src="figures/flex1.png">
</p>

In this work we present Flexagon, the first SpMSpM reconfigurable accelerator that is capable of performing SpMSpM computation by using the particular dataflow that best matches each case. Flexagon accelerator is based on a novel Merger-Reduction Network (MRN) that unifies the concept of reducing and merging in the same substrate, increasing efficiency. Additionally, Flexagon also includes a 3-tier memory hierarchy, specifically tailored to the different access characteristics of the input and output compressed matrices.

<p align="center">
 <img src="figures/flex2.png">
</p>

We model Flexagon in [SST-STONNE](SST.md).
