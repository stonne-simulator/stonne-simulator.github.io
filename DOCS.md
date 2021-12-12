#### [HOME][README.md]

# Documentation

## STONNE USER INTERFACE. HOW TO RUN STONNE QUICKLY.

The STONNE User Interface facilitates the execution of STONNE. Through this mode, the user is presented with a prompt to load any layer and tile parameters onto a selected instance of the  simulator, and runs it with random tensors. 


**INSTALLATION**

The installation of STONNE, along with its user interface,  can be carried out by typing the next commands:
```
cd stonne
make all
```
These commands will generate a binary file 'stonne/stonne'. This binary file can be executed to run layers and gemms with any dimensions and any hardware configuration. All the tensors are filled using random numbers. 

**HOW TO RUN STONNE**

Currently, STONNE runs 4 types of operations: Convolution Layers, FC Layers, Dense GEMMs and Sparse GEMMs. Please, note that almost any kernel can be, in the end, mapped using these operations. Others operations such as pooling layers will be supported in the future. However, these are the operations that usually dominate the execution time in machine learning applications. Therefore, we believe that they are enough to perform a comprehensive and realistic exploration. Besides, note that a sparse convolution might be also supported as all the convolution layers can be converted into a GEMM operation using the im2col algorithm.

The sintax of a STONNE user interface command to run any of the available operations is as follows:
```
./stonne [-h | -CONV | -FC | -SparseGEMM | -DenseGEMM] [Hardware parameters] [Dimension and tile Parameters]
```


[HELP MENU]

A help menu will be shown when running the next command:

```
./stonne -h: Obtain further information to run STONNE
```

[Hardware parameters]

The hardware parameters are common for all the kernels. Other parameters can be easily implemented in the simulator. Some parameters are tailored to some specific architectures.


*-num_ms*=[x]: Number of multiplier switches (must be power of 2) (Flexible architecture like MAERI or SIGMA)

*-dn_bw*=[x]: Number of read ports in the SDMemory (must be power of 2) (All architectures)

*-rn_bw*=[x]: Number of write ports in the SDMemory (must be power of 2) (All architectures)

*-rn_type*=[0=ASNETWORK, 1=FENETWORK, 2=TEMPORALRN]: type of the ReduceNetwork to be used (Not supported for SparseGEMM)

*-mn_type*=[0=LINEAR, 1=OS\_MESH]: Type of Multiplier network to be used. Linear is for flexible architectures, OS\_MESH for rigid architectures like TPU.

*-mem\_ctrl*=[MAERI\_DENSE\_WORKLOAD, SIGMA\_SPARSE\_GEMM, TPU\_OS\_DENSE]": type of memory controller to be used

*-accumulation_buffer*=[0,1]: enables the accumulation buffer. Mandatory in Rigid architectures.

*-print_stats*=[0,1]: Flag that enables the printing of the statistics


[Dimension and tile Parameters]

Obviously, the dimensions of the kernel depends on the type of the operation that is going to be run. Next, it is described the different parameters according to each supported operation:

[CONV]

*-layer_name*: Name of the layer to run. The output statistic file will be named accordingly

*-R*=[x]: Number of flter rows

*-S*=[x]: Number of filter columns

*-C*=[x]: Number of filter and input channels

*-K*=[x]: Number of filters and output channels

*-G*=[x]: Number of groups

*-N*=[x]: Number of inputs (Only 1 is supported so far)

*-X*=[x]: Number of input rows

*-Y*=[x]: Number of input columns

*-strides*=[x]: Stride value used in the layer

*-T_R*=[x]: Number of flter rows mapped at a time

*-T_S*=[x]: Number of filter columns mapped at a time

*-T_C*=[x]: Number of filter and input channels per group mapped at a time

*-T_K*=[x]: Number of filters and output channels per group mapped at a time

*-T_G*=[x]: Number of groups mappd at a time

*-T_N*=[x]: Number of inputs mapped at a time (Only 1 is supported so far)

*-T_X_*=[x]: Number of input rows mapped at a time

*-T_Y_*=[x]: Number of input columns mapped a time

Please make sure that these next constraints are followed (i.e., tile dimension must be multiple of its dimension):

If the architecture to be run is flexible (MAERI or SIGMA):

 -T_R % R = 0; -T_S % S = 0; -T_C % C = 0 ;-T_K % K = 0; -T_G % G = 0; -T_X_ % ((X - R + strides) / strides) = 0; -T_Y_ % ((Y - S + strides) / strides) = 0;



[FC]

*-layer_name*=[str]: Name of the layer to run. The output statistic file will be called by this name

*-M*=[x]: Number of output neurons

*-N*=[x]: Batch size

*-K*=[x]: Number of input neurons

*-T_M*=[x]: Number of output neurons mapped at a time

*-T_N*=[x]: Number of batches mapped at a time

*-T_K*=[x]: Number of input neurons mapped at a time

[DenseGEMM]

*-layer_name*=[str]: Name of the layer to run. The output statistic file will be called by this name

*-M*=[x]: Number of rows MK matrix

*-N*=[x]: Number of columns KN matrix

*-K*=[x]: Number of columns MK and rows KN matrix (cluster size)

*-T_M*=[x]: Number of M rows mapped at a time

*-T_N*=[x]: Number of N columns at a time

*-T_K*=[x]: Number of K elements mapped at a time

[SparseGEMM]

*-layer_name*=[str]: Name of the layer to run. The output statistic file will be called by this name

*-M*=[x]: Number of rows MK matrix

*-N*=[x]: Number of columns KN matrix

*-K*=[x]: Number of columns MK and rows KN matrix (cluster size)

*-MK_sparsity*=[x]: Percentage of sparsity MK matrix (0-100)

*-KN_sparsity*=[x]: Percentage of sparsity KN matrix (0-100)

*-dataflow*=[MK_STA_KN_STR | MK_STR_KN_STA]

*-optimize*=[0,1]: apply compiler-based optimizations

**RUNNING EXAMPLES**

Example running a convolution layer: 
```
./stonne -CONV -R=3 -S=3 -C=6 -G=1 -K=6 -N=1 -X=20 -Y=20 -T_R=3 -T_S=3 -T_C=1 -T_G=1 -T_K=1 -T_N=1 -T_X_=3 -T_Y_=1 -num_ms=64 -dn_bw=8 -rn_bw=8
```

Example running a FC layer:
```
./stonne -FC -M=20 -N=20 -K=256 -num_ms=256 -dn_bw=64 -rn_bw=64 -T_K=64 -T_M=2 -T_N=1
```

Example of running a sparse GEMM:
```
./stonne -SparseGEMM -M=20 -N=20 -K=256 -num_ms=128 -dn_bw=64 -rn_bw=64  -MK_sparsity=80 -KN_sparsity=10 -dataflow=MK_STA_KN_STR
```

Example of running a dense GEMM:
```
/stonne -DenseGEMM -M=20 -N=20 -K=256 -num_ms=256 -dn_bw=64 -rn_bw=64 -T_K=64 -T_M=2 -T_N=1
```

Example of running a dense GEMM over TPU:
```
./stonne -DenseGEMM -M=4 -N=4 -K=16 -ms_rows=4 -ms_cols=4 -dn_bw=8 -rn_bw=16  -T_N=4 -T_M=1 -T_K=1 -accumulation_buffer=1 -rn_type="TEMPORALRN" -mn_type="OS_MESH" -mem_ctrl="TPU_OS_DENSE"
```

**OUTPUT** 

Every layer execution generates 2 files in the path in which the simulator has been executed (the env variable OUTPUT_DIR can be set to indicate another output path): 

- A json file with all the hardware statistics generated during the execution. 

- A counters file with the number of use of every component of the architecture generated. This can be utilized to generate the energy model. 

Note that after the execution, the results obtained in the output tensor by the simulator are compared with a  CPU algorithm to ensure the correctness of the simulator. Note that if the simulator does not output the correct results, an assertion will raise at the end of the execution. 



**Generating Energy Numbers**

In order to generate the energy consumption of the execution we have developed a Python script that takes in the counters file generated during the execution and a table-based energy model. The script is located in energy_tables folder and can be run by means of the next command:

```
./calculate_energy.py [-v] -table_file=<Energy numbers file> -counter_file=<Runtime counters file> -[out_file=<output file>]
```
The current energy numbers are located in the file energy_tables/energy_model.txt. We obtained the energy numbers through synthesis using Synopsys Design-Compiler and place-and-route using Cadence Innovus on each module inside the MAERI and SIGMA RTL to populate the table. Users can plug in the numbers for their own implementations as well.

## PYTHON FRONTEND

At this point, the user must be familiar with the usage of STONNE and the set of statistics that the tool is able to output. However, with the STONNE user interface presented previously, the user must have realised that the inputs and outputs coming in and out in the simulator are random. Here, it is explained how to run real DNN models using pytorch and STONNE as a computing device. 

The pytorch-frontend is located in the folder 'pytorch-frontend' and this basically contains the Pytorch official code Version 1.7 with some extra files to create the simulation operations and link them with the 'stonne/src' code. The current version of the frontend is so well-organized that running a pytorch DNN model on STONNE is straightforward. 

**INSTALLATION**

Installing Pytorch-frontend will make the same effort as installing the original Pytorch framework (see 'pytorch-frontend/README.md' or access to their original repository). 

First, you will need Python 3.6 or later and a C++14 compiler. Also, we highly recommend installing an Anaconda environment. Once you have Anaconda installed (https://www.anaconda.com/products/individual) you can proceed to the installation. Next, we summarize the installation process on Linux (Please refer to the original Pytorch documentation to learn how to install it in other operating system):

```
cd pytorch-frontend/
export CMAKE_PREFIX_PATH=${CONDA_PREFIX:-"$(dirname $(which conda))/../"}
python setup.py install
```
Next, run the next commands, which will install the pytorch_stonne package, that will be used for the connection with STONNE.
```
cd stonne_connection/
python setup.py install
```
Besides, you will need the next dependencies in order to run all the benchmarks:

- torchvision (https://github.com/pytorch/vision)
- transformers (https://github.com/huggingface/transformers)
- numpy

Please, make sure all the dependencies are solved before running any benchmark. Besides, make sure the installation of the dependencies do not remove the current version of pytorch to install another one. This is very common and can be frustrating. In order to avoid so, I recommend installing the torchvision package and transformers from sources. Here an example of installing torchvision from source:

```
git clone https://github.com/pytorch/vision
cd vision
python setup.py install
```

Please, follow the official instructions for each dependency if something occurs. 

**RUNNING PYTORCH IN STONNE**

Running pytorch using STONNE as a device is almost straightforward.
Let's assume we define a DNN model using pytorch. This model is composed of a single and simple convolutional layer. Next, we present this code:

```python
class Net(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.Conv2d(5,5,5, groups=1) # in_channels=5, out_channels=5, filter_size=5
    def forward(self, x):
        x = self.conv1(x)
        return x
```

This code can be easily run in CPU just by means of creating an object of type Net and running the forward method with the correct tensor shape as input.

```python
net = Net()
print(net)
input_test = torch.randn(5,50,50).view(-1,5,50,50)
result  = net(input_test)
```

Migrating this model to STONNE is as simple as turning the Conv2d operation into a SimulatedConv2d operation. Next, we can observe an example:
```python
class Net(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv1 = nn.SimulatedConv2d(5,5,5,'$PATH_TO_STONNE/simulation_files/maeri_128mses_128_bw.cfg', 'dogsandcats_tile.txt', sparsity_ratio=0.0, stats_path='.', groups=1) 
    def forward(self, x):
        x = self.conv1(x)
        return x
```

As we can see, we have inserted 4 new parameters:

- sim_file (str): This is the path to the configuration file of STONNE. This file defines the hardware to be simulated in every execution of STONNE. You can see multiple examples in the folder 'simulation_files'.

- tile (str): This is the path to a file that defines the tile to be used to partition that layer. An example of this file might be found in 'minibenchmarks/dogsandcats_tile.txt' (Note that an example for a linear tile file might be found in 'minibenchmarks/dogsandcats_tile_fc.txt'). This parameter only will make sense if the hardware configuration file contains a dense memory controller. If the memory controller is sparse, then the execution will not require tiling as it is explained in SIGMA paper.

- sparsity_ratio (float 0.0-1.0): This is the sparsity ratio used to prunne the weight tensor. This parameter only makes sense if a sparsity controller is used in the hardware configuration file. Otherwise this will be ignored.  They way to proceed in the current version of STONNE is indicating this parameter. Then, previously to the simulation, the weight tensor is prunned accordingly to that parameter and the bitmaps are created accordingly. Note that the weights are not retrained and therefore this will affect to the accuracy of the model. However, in terms of a simulation perspective, this lower accuracy is not affected at all. Obviously, this is a way to proceed. It is possible, with low efforts, to run an already prunned and re-trained model. To do so, the code have to be briefly modified to remove the prunning functions and use the real values as they are. By the moment, STONNE only allows bitmap representation of sparsity. If you have a model with other compression format, you could either code your own memory controller to support it or code a simple function to turn your representation format into a bitmap representation. 

- stats_path: This is an optional parameter and points to a folder in which the stats of the simulation of that layer will be stored. 


The addition of these 4 parameters and the modification of the function will let pytorch  run the layer in STONNE obtaining the real tensors.

In the current version of the pytorch-frontend we also support nn.SimulatedLinear and torch_stonne.SimulatedMatmul operations that correspond with both nn.Linear and nn.Matmul operations in the original pytorch framework. The only need is to change the name of the functions and indicate the 3 extra parameters. 


**RUNNING REAL BENCHMARKS**  

In order to reduce the effort of the user, we have already migrated some models to STONNE. By the moment, we have 4 DNN benchmarks in this framework: Alexnet, SSD-mobilenets, SSD-Resnets1.5 and BERT. All of them are in the folder 'benchmarks'. Note that to migrate these models, we have had to understand the code of all of them, locate the main kernels (i.e., convolutions, linear and matrix multiplication operations) and turn the functions into the simulated version. That is the effort you require to migrate a new model. We will update this list over time. 

Running these models is straightforward as we have prepared a script ('benchmarks/run_benchmarks.py file') that performs all the task automatically. Next, we present one example for each network:

```
cd benchmarks
```

- Running BERT:  
```
python run_benchmarks.py "bert" "../simulation_files/sigma_128mses_64_bw.cfg" "NLP/BERT/tiles/128_mses/" "0.0" ""
```

- Running SSD-Mobilenets
```
python run_benchmarks.py "ssd_mobilenets" "../simulation_files/sigma_128mses_64_bw.cfg" "object_detection/ssd-mobilenets/tiles/128_mses" "0.0" ""
```

- Running SSD-Resnets:
```
 python run_benchmarks.py "ssd_resnets" "../simulation_files/sigma_128mses_64_bw.cfg" "object_detection/ssd-mobilenets/tiles/128_mses" "0.0" "" 
```
