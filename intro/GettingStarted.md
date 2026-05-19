

## Setup and Installation

To install / setup your environment to run the code for this project we need to

* [Download the data](https://github.com/alvarovm/solarcelldata/wiki/Getting-Started#download-the-data)
* Install the DyeDB python package which we describe below.

### Clone the repo
### In NERSC computers
1. Clone the [Githhub repo](https://github.com/alvarovm/solarcelldata)

In Jypyter-lab, open a  new Terminal and run:
```
git config --global --add safe.directory /global/cfs/cdirs/m4388/Project7/solarcelldata/.git
git clone /global/cfs/cdirs/m4388/Project7/solarcelldata
cd ./solarcelldata
```


2. Create a virtual environment with [conda](anaconda.com)
```
module add python
conda activate /global/common/software/m4388/solarcelldata/
```



#### Outside NERSC
1. Clone the [Githhub repo](https://github.com/alvarovm/solarcelldata)
```
git clone https://github.com/alvarovm/solarcelldata.git
cd ./solarcelldata
```
2. Create a virtual environment with [conda](anaconda.com)
```
conda create -f environment.yml
conda activate solarcelldata
```

## Working with Jupyter server at NERSC, August 2025

For this project we will use [Jupyter notebooks](https://jupyter.readthedocs.io/en/latest/).
 
The documentation for using Jupyter at NERSC could found here [https://docs.nersc.gov/services/jupyter/](https://docs.nersc.gov/services/jupyter/).

Here is a link to NERSC's JupyterHub service: [https://jupyter.nersc.gov/hub/home](https://jupyter.nersc.gov/hub/home)

During the IntroToHPC2025 bootcamp we have scheduled reservations for all the attendees during the following hours:
* Mon. 20 GPU nodes, 2:30 pm - 5:30 pm, reservation name: intro_hpc_day1
* Tue, 40 GPU nodes, 3:00 pm - 5:30 pm,  reservation name: intro_hpc_day2_pm
* Wed, 40 GPU nodes, 9:00 am - 5:30 pm,  reservation name: intro_hpc_day2_pm
* Thur, 40 GPU nodes, 12:00 pm - 8:30 pm,  reservation name: intro_hpc_day4 

Each GPU node has 64 CPU cores (with 2 hyperthreads per physical core, so Slurm sees it as having 128 logical cores, or Slurm says it has 128 CPUs,) and 4 GPUs. The GPU nodes are shareable. 

For this project each student can request 1/4 of a GPU node, having access to 32 logical CPUs and 1 GPU:
* Running a Jupyter notebook server on a shared GPU node, but only using 1 GPU:
* Select m4388 for account
* Change QOS to "shared"
* Change cpus-per-task to 32
* Change gpus-per-task to 1
* Set reservation to _reservation on that day_

**Notice**  that not all the exercises and notebooks would be benefited of GPUs.

Once you opened a jupyter server, then you can open a terminal to modify your files, or browse your files (left menu) and open jupyter notebooks  (files ending with the extension .ipynb).

We have prepared a specific kernel for the materials of this project. The kernel selections is found in the upper right corner of the notebook, and it will look like (select **solarcelldata**):

![](https://github.com/alvarovm/solarcelldata/blob/cf6a2df7c123875713066bdbe391c6dc58a9ee4b/figures/kernel.png)

Tutorial video:



[![Jupyter notebook Dye Sensitized Solar Cell](http://img.youtube.com/vi/4EjLHBAEknc/0.jpg)](https://www.youtube.com/watch?v=4EjLHBAEknc "Opening a notebook in NERSC")



## Tools for working with molecules

Molecules are just a set of ordered atoms. Depending on how those atoms are positioned the molecules have different properties.

### Molecules from literature

We used [ChemDataExtractor](http://chemdataextractor.org/) to retrive molecules and their properties from scientific papers. You can try 

You could find an example [here](https://github.com/mcs07/ChemDataExtractor/blob/master/examples/extracting_a_custom_property.ipynb).

### Installation of chemdataextractor (optional)
```
conda create -n chemdata python==3.8 pip
conda activate chemdata
pip install chemdataextractor
cde data download
```

### Chemical Identifier Resolver

The National Institute of Health provides an online name resolver ([Cactus](https://cactus.nci.nih.gov/chemical/structure)) that we use to convert molecular names in chemical formular or identifiers.
This is used quering to an web address like: `https://cactus.nci.nih.gov/chemical/structure/"structure identifier"/"representation"`

For example, the address composed for the molecule [aspirin](https://en.wikipedia.org/wiki/Aspirin) would look like:
`https://cactus.nci.nih.gov/chemical/structure/aspirin/smiles`
and this will result in a SMILES string:
`CC(=O)Oc1ccccc1C(O)=O`

### Molecules in SMILES format

Molecules could be represented in many ways (eg. names, sketches, 3D models with balls and sticks, etc.). One useful way is the [simplified molecular-input line-entry system or SMILES](https://en.wikipedia.org/wiki/Simplified_molecular-input_line-entry_system). SMILES are helpful to represent molecules in 2D as string of characters.
This representation cannot provide information about distances or angles among atoms, also there there is not a unique way to describe a molecule with SMILES. Nevertheless, SMILES are very useful to create molecules and or get an idea of its composition.

You can visualize the aspirine's SMILES in the follow link: [https://cactus.nci.nih.gov/chemical/structure/CC(=O)Oc1ccccc1C(O)=O/image](https://cactus.nci.nih.gov/chemical/structure/CC(=O)Oc1ccccc1C(O)=O/image)


![](https://github.com/alvarovm/solarcelldata/blob/e94ae24f502622ff975423f8532be04ce8aa8628/figures/aspirin.gif)





