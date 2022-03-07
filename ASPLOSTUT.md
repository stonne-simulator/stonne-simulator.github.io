#### [HOME](README.md) &nbsp; &nbsp; &nbsp; [Features](FEATURE.md) &nbsp; &nbsp; &nbsp;  [Documentation](DOCS.md) &nbsp; &nbsp; &nbsp;  [ASPLOS 2022 Tutorial](ASPLOSTUT.md) &nbsp; &nbsp; &nbsp;  [GNN-Dataflow](GNN.md)&nbsp; &nbsp; &nbsp;  [Codebase](https://github.com/stonne-simulator/stonne) &nbsp; &nbsp; &nbsp; [Resources](RESOURCES.md)


<h1 align=center>ASPLOS 2022 STONNE Tutorial</h1>

Date: 1st March 2022

## ORGANIZERS

[Francisco Muñoz-Martínez](https://www.linkedin.com/in/fr-munoz-mrtinez/) (Universidad de Murcia) <br>
[Raveesh Garg](https://www.linkedin.com/in/raveesh-garg-61b47b150/) (Georgia Institute of Technology) <br>
[Tushar Krishna](https://tusharkrishna.ece.gatech.edu) (Georgia Institute of Technology) <br>
[José L. Abellán](https://sites.google.com/ucam.edu/jlabellan/) (Universidad Católica de Murcia) <br>
[Manuel E. Acacio](https://webs.um.es/meacacio/) (Universidad de Murcia) <br>

## OVERVIEW

The design of specialized architectures for accelerating the inference procedure of Deep Learning (DL) is a booming area of research nowadays. 
While first-generation systolic-based accelerator proposals used simple fixed dataflows tailored for dense Deep Neural Networks (DNNs) applications, more recent architectures like MAERI or SIGMA have argued for flexibility to efficiently support a wide variety of layer types, dimensions, and sparsity. In addition, the recent appearance of Graph Neural Networks (GNNs) applications has resulted into multi-phase accelerators that combine the execution of multiple kernels in a pipelined manner, making the architectures much more complex. 

As the complexity of these accelerators grows, the analytical models currently being used for design-space exploration are unable to capture execution-time subtleties, leading to inexact results in many cases. This opens up a need for cycle-level simulation tools to allow for fast and accurate design-space exploration of DL accelerators, and rapid quantification of the efficacy of architectural enhancements during the early stages of a design. To this end, STONNE (Simulation Tool for Neural Network Engines) is a cycle-level microarchitectural simulation framework that can plug into any high-level DL framework as an accelerator device and perform full-model evaluation (i.e. we are able to simulate real, complete, unmodified DNN models) of state-of-the-art systolic and flexible DNN accelerators, both with and without sparsity support. STONNE is developed by the University of Murcia and the Georgia Institute of Technology and is open-sourced under the terms of the MIT license.

In this tutorial we demonstrate how STONNE enables research on DNNs accelerators by means of several use cases that range from the microarchitectural networks on-chip present in DNN accelerators to the scheduling strategies that can be utilized to improve energy efficiency in sparse accelerators. Further, we present OMEGA, another framework built on top of STONNE that enables the exploration of dataflows for accelerators for multi-phase GNN applications which are gaining popularity in the AI and HPC community.

<p align="center">
      <img src="figures/Top_Level_Stonne_shorter.png">
     <img src="figures/omega.png" height="50%" width="50%">
</p>

The figures above give a brief overview of STONNE and OMEGA frameworks.

Figure on the left shows a high-level view of STONNE with its three major modules for full-model simulation flows. The input module feeds the simulator with the values to be computed along with the DNN dataflows and the hardware configuration. Then, the simulation engine performs the cycle-level simulation using its internal simulated microarchitectural building blocks. Finally, the output module returns the statistics of the simulation. For more details, please refer to the [features of STONNE](FEATURE.md).

Figure on the right shows a brief overview of OMEGA framework built on top of STONNE. OMEGA computes GNNs which consist of an SpMM followed by a GEMM phase. STONNE simulator accurately computes the timestamps and buffer accesses for individual phases and these are fed into the inter-phase cost model which computes the final metrics considering the inter-phase (between the phases) dataflow/pipelining strategies. Please refer to the [GNN Dataflows](GNN.md) page for more details.

## SCHEDULE

The planned schedule is as follows

|   Time ET   |  Time CET   | Agenda                                                          |Presenter         | Slides |
| ------------|-------------|-----------------------------------------------------------------|------------------|--------|
| 8:00-8:40   | 14:00-14:40 | DNN and GNN Accelerators; Flexible Accelerators                 |Tushar            |  [Link](https://github.com/stonne-simulator/stonne-simulator.github.io/blob/main/slides/ASPLOS_STONNE_Tutorial_Part1.pdf)   |
| 8:40-9:10   | 14:40-15:10 | Cycle accurate simulation motivation and Overview of STONNE     |Jose Luis         |  [Link](https://github.com/stonne-simulator/stonne-simulator.github.io/blob/main/slides/ASPLOS_STONNE_Tutorial_Part2.pdf)  |
| 9:10-9:40   | 15:10-15:40 | (Hands-on) Simulating flexible DNN Accelerators                 |Francisco         |  [Link](https://github.com/stonne-simulator/stonne-simulator.github.io/blob/main/slides/ASPLOS_STONNE_Tutorial_Part3.pdf)  |       
| 9:40-10:10  | 15:40-16:10 | (Hands-on) Research usecase of STONNE - Skipping zeros          |Francisco         |  [Link](https://github.com/stonne-simulator/stonne-simulator.github.io/blob/main/slides/ASPLOS_STONNE_Tutorial_Part3.pdf)  |
| 10:10-10:40 | 16:10-16:40 | Coffe Break                                                                                 |
| 10:40-11:15 | 16:40-17:15 | (Hands-on) Adding new operations in STONNE - Conv1D             |Francisco         |  [Link](https://github.com/stonne-simulator/stonne-simulator.github.io/blob/main/slides/ASPLOS_STONNE_Tutorial_Part3.pdf)  |
| 11:15-11:45 | 17:15-17:45 | GNN Dataflow Taxonomy and (Demo) OMEGA framework.               |Raveesh           |  [Link](https://github.com/stonne-simulator/stonne-simulator.github.io/blob/main/slides/ASPLOS_STONNE_Tutorial_Part4.pdf)  |
| 11:45-12:00 | 17:45-18:00 | Roadmap for future research/development                         |Manuel E.         |  [Link](https://github.com/stonne-simulator/stonne-simulator.github.io/blob/main/slides/ASPLOS_STONNE_Tutorial_Part5.pdf)  |

## Docker Image
To follow the tutorial, we have created a docker image which contains both STONNE and OMEGA tools already installed. We encourage attendees to download and set up the image before the tutorial. To do that, just introduce the next docker command: 

```
docker run -it franciscomunoz/stonne_omega_img /bin/bash
```


## CODEBASES

### STONNE

https://github.com/stonne-simulator/stonne

### OMEGA

https://github.com/stonne-simulator/omega

## PUBLICATIONS

### STONNE

Francisco Muñoz-Martínez, José L. Abellán, Manuel E. Acacio, and Tushar Krishna. Stonne: Enabling cycle-level microarchitectural simulation for dnn inference accelerators.  In 2021 IEEE International Symposium on Workload Characterization (IISWC), 2021. [(pdf)](https://arxiv.org/pdf/2006.07137.pdf)

### OMEGA

Raveesh Garg, Eric Qin, Francisco Muñoz-Martínez, Robert Guirado, Akshay Jain, Sergi Abadal, José L Abellán, Manuel E Acacio, Eduard Alarcón, Sivasankaran Rajamanickam, and Tushar Krishna.  Understanding the Design-Space of Sparse/Dense Multiphase GNN dataflows on Spatial Accelerators. In 2022 IEEE International Parallel and Distributed Processing Symposium (IPDPS), 2022. [(pdf)](https://arxiv.org/pdf/2103.07977)

## CITATIONS

If you use STONNE or OMEGA framework in your research. Please cite-

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


## CONTACT
Francisco Muñoz-Martínez - francisco.munoz2@um.es

## ACKNOWLEDGMENTS
This work was supported by NEC Laboratories Europe, Project grant PID2020-112827GB-I00, by MCIN/AEI 10.13039/501100011033, RTI2018-098156-B-C53 (MCIU/AEI/FEDER,UE), NSF OAC 1909900 and US Department of Energy ARIAA co-design center. F. Muñoz-Martínez was supported by grant 20749/FPI/18 from Fundación Séneca.
