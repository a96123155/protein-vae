# Variational autoencoder for protein sequences

This repository provides code to accompany the paper:

Greener JG, Moffat L and Jones DT, Design of metalloproteins and novel protein folds using variational autoencoders, 2018 - [link to pre-print](https://arxiv.org/abs/1806.09900)

The work describes a variational autoencoder that can add metal binding sites to protein sequences, or generate protein sequences for a given protein topology.

## Getting Started
This instructions will get a copy of this repo on your system and running so you can produce sequences with our models or expand on our work. These files have been cleaned and minimized so they can be run using python with as few dependencies as possible

### Dependencies
The follow are the packages needed to run our software. Our software uses python and leverages several python 
* [python 3.6](https://www.python.org/)
* [pytorch 4.0](https://pytorch.org/)
* [sklearn 0.19.1](http://scikit-learn.org/stable/)
* [numpy 1.14.1](http://www.numpy.org/)
* [lark-parser 0.4.1](https://github.com/lark-parser/lark)

Follow the links to install the correct version of all of the packages. Note that installing pytorch can be a bit tricky but all of these packages can be easily install with [Anaconda](https://anaconda.org/). Using the `conda install` command from the Anaconda package is highly recommended. The lark-parser package is only needed if you wish to generate protein sequences using the model that takes in a strucure grammar to produce sequences (described more below). The neural networks are built using pytorch and some utility functions from sklearn are used for metrics, making proper installation critical. 

If you wish to extend our work note that we have configured the general use scripts to run solely on cpu as they do not require the computational power of a GPU. However, the example training scripts provided can be run on GPU and contain switches and/or command line arguments to enable this. 

### Installation

Installing our software is incredibly easy. Navigate to the directory you wish to store the software in and simply



## Usage

The two tasks this work approaches are adding a metal binding site to a protein sequence and generating a protein sequence for a given topology string. The first is described as Task 1 and the second as Task 2. Below are descriptions for using the trained models.

### Task 1
This is adding a metal binding site to a protein sequence. The files for this task are located in the metal_gen folder. Aside from the model file in the folder folder, the main script is the `metal_VAE_pytorch.py`. This file can be run from the command line with several arguments that can be seen by looking at the source. The code itself contains explanations for its use, but more specifically, it can be used for training a model or producing samples of a protein similar to another protein provided.

Note the model provided and thus inference was trained and can be used with the `nostruc` dataset.

<b> Training </b>

In the case of training please get in contact for the datasets, however if you have correctly formated data you should be able to use that instead. A model has been provided for a network with a latent dimension of 16 which was used in the paper. The dataset can be specified by command line args as either `struc` or `nostruc`. These are numpy `.npy` files of size `L x 4353` and `L x 3088` where `L` is the number of examples in the dataset. Both datasets have one-hot encoded sequences as the first 3080 dimensions and the next 8 as the binary switches for metal binding. The `struc` dataset contains an extra 1265 dimensions that describe the input data.  

Read through the command line arguments in the files in order to understand what parameters of the network can be changed, but you can change them by changing the defaults if running in an IDE. The current defaults are what was used to train the 16 dimensional network. To train the network you also need to change the switches specified in the script so that `cuda=True` and `train=True`.

<b> Sampling a new metal binder </b>

First make sure that that `cuda=False` and `train=False` in the script. The network is capable of running inference very quickly on just a cpu. From there make sure that `new_metal=True`. At the bottom of the script you will see

```Python
if new_metal:
    name="prots_nomet"
    ...
```
The `name` is the name not including `.npy` of the file being used (that needs to be placed on the current directory) of protein(s) you wish to alter. So the input file will be a numpy binary of size `L x 3088` where the last 8 metal binding flags are changed in accordance with what you want metal binding you want to add. If you've got that sorted then just run the script.

<b> Encoding and decoding separately</b>

In order to do these just make a function that is a part of the network module that looks like the forward pass but only uses the layers you intend of using i.e. for decoding or encoding. From there add whatever you like at the end of the script to call these functions and encode or decode data.

### Task 2
This is generating a protein sequence for a given topology string and the scripts are located in the fold_gen folder.
