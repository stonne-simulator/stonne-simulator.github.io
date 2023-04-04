#### [HOME](README.md) &nbsp; &nbsp; &nbsp; [Features](FEATURE.md) &nbsp; &nbsp; &nbsp;  [Docs](DOCS.md) &nbsp; &nbsp; &nbsp; [ASPLOS 2023 Tutorial](ASPLOSTUT.md)  &nbsp; &nbsp; &nbsp;  [GNN-Dataflow](GNN.md)&nbsp; &nbsp; &nbsp;  [Code](https://github.com/stonne-simulator/stonne) &nbsp; &nbsp; &nbsp; [Resources](RESOURCES.md) &nbsp; &nbsp; &nbsp; [SST Integration](SST.md) &nbsp; &nbsp; &nbsp; [Flexagon](FLEXAGON.md)

<p align="center">
 <img src="figures/stonne-logo.png" height="25%" width="25%">
</p>

# STONNE: A Simulation Tool for Neural Networks Engines 

## WHAT IS STONNE
The design of specialized architectures for accelerating the inference of Deep Neural Networks (DNNs) is a booming area of research nowadays. While first-generation accelerator proposals used simple fixed dataflows tailored for 
dense DNNs, more recent architectures have argued for flexibility to efficiently support a wide variety of layer types, dimensions, and sparsity. As the complexity of these accelerators grows, it becomes more and more appealing for researchers to have cycle-level simulation tools at their disposal to allow for fast and accurate design-space exploration, and rapid quantification of the efficacy of architectural enhancements during the early stages of a design. To this end, we present STONNE (Simulation TOol of Neural Network Engines), a cycle-level, highly-modular and highly-extensible simulation framework that can plug into any high-level DNN framework as an accelerator device and perform end-to-end evaluation of flexible accelerator microarchitectures with sparsity support, running complete DNN models.

<p align="center">
 <img src="figures/Top_Level_Stonne_shorter.png">
</p>


STONNE is a cycle-level microarchitectural simulator for flexible DNN inference accelerators. To allow for end-to-end evaluations, the simulator is connected with a Deep Learning (DL) framework (Caffe and Pytorch DL frameworks in the current version). Therefore, STONNE can fully execute any dense and sparse DNN models supported by the DL framework that uses as its front-end.
 The simulator has been written entirely in C++, following the well-known GRASP and SOLID programming principles of object-oriented design. This has simplified its development and makes it easier the implementation of any kind of DNN inference accelerator microarchitecture, tile configuration mappings and dataflows.

The figure presented above shows a high-level view of STONNE with the three major modules involved in the end-to-end simulation flow.

For more details on the API and the features, please refer to the [following link](FEATURE.md).

STONNE simulator can also be called from within PyTorch. Please refer to the [documentation](DOCS.md).

STONNE simulator can be modified to enable research beyond just DNN to other applications like Graph Neural Networks.

For a detail research usecase for Graph Neural Network (GNN) Dataflow Analysis and the [***OMEGA***](https://github.com/stonne-simulator/omega) GNN dataflow cost modelling framework built on STONNE, please check [GNN-Dataflow usecase](GNN.md) and [the corresponding GNN-Datalow paper](https://arxiv.org/abs/2103.07977)

STONNE is also integrated with SST, to model detailed memory hierarchy ([SST-STONNE webpage](SST.md))([Github](https://github.com/stonne-simulator/sst-elements-with-stonne)).

We propose a novel sparse accelerator Flexagon which models Inner-Product, Outer Product and Gustavsons dataflows in a single substrate. It is modeled in SST-STONNE. ([Flexagon webpage](FLEXAGON.md))([Paper pdf](https://arxiv.org/pdf/2301.10852.pdf))

## BIBTEX CITATIONS

If you use STONNE or OMEGA framework in your research or if you run on flexagon accelerator model in SST-STONNE. Please cite-

### STONNE

```
@INPROCEEDINGS{STONNE21,
  author =       {Francisco Mu{\~n}oz-Mart{\'i}nez and Jos{\'e} L. Abell{\'a}n and Manuel E. Acacio and Tushar Krishna},
  title =        {STONNE: Enabling Cycle-Level Microarchitectural Simulation for DNN Inference Accelerators},
  booktitle =    {2021 IEEE International Symposium on Workload Characterization (IISWC)}, 
  year =         {2021},
  volume =       {},
  number =       {},
  pages =        {},
}
```

### OMEGA

```
@inproceedings{garg2021understanding,
  title={Understanding the Design-Space of Sparse/Dense Multiphase GNN dataflows on Spatial Accelerators},
  author={Garg, Raveesh and Qin, Eric and Mu{\~n}oz-Mart{\'\i}nez, Francisco and Guirado, Robert and Jain, Akshay and Abadal, Sergi and Abell{\'a}n, Jos{\'e} L and Acacio, Manuel E and Alarc{\'o}n, Eduard and Rajamanickam, Sivasankaran and Krishna, Tushar},
  booktitle={2022 IEEE International Parallel and Distributed Processing Symposium (IPDPS)},
  year={2022}
}
```

### Flexagon

```
@inproceedings{munoz2023flexagon,
  title={Flexagon: A Multi-Dataflow Sparse-Sparse Matrix Multiplication Accelerator for Efficient DNN Processing},
  author={Mu{\~n}oz-Mart{\'\i}nez, Francisco and Garg, Raveesh and Pellauer, Michael and Abell{\'a}n, Jos{\'e} L and Acacio, Manuel E and Krishna, Tushar},
  booktitle={Proceedings of the 28th ACM International Conference on Architectural Support for Programming Languages and Operating Systems, Volume 3},
  pages={252--265},
  year={2023}
}
```

## Contact
For any questions or issues related to the simulator, submit an issue at https://github.com/stonne-simulator/stonne/issues.
