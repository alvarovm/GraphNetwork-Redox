
## Pandas cookbook

`Pandas` python module is a very powerful package to manipulate large amounts of data in a spreadsheet-like format. In this project, we will load out data and store as a `DataFrame` object, which is how `pandas` allocate data in the memory.

This code snippet demonstrates how to import the `pandas` and `numpy` libraries in Python to manipulate and analyze data. It begins by importing these libraries for data manipulation and numerical operations, which are essential tools for data science and numerical computation:

```python
import pandas as pd
import numpy as np
```

Let's create and initialize a nested list with numerical values, this can be seen as a matrix.

```python
values_list = [[15, 2.5, 100], [20, 4.5, 50], [25, 5.2, 80],
               [45, 5.8, 48], [40, 6.3, 70], [41, 6.4, 90],
               [51, 2.3, 111]]
```

Now, this matrix will be transferred as `DataFrame`.

```python
# Creating a pandas DataFrame from the nested list
df = pd.DataFrame(values_list, columns=['Field_1', 'Field_2', 'Field_3'],
                  index=['a', 'b', 'c', 'd', 'e', 'f', 'g'])
```
One can printout the the `DataFrame` and visualize as a spreadsheet.

```python
df
```

### Applying a function to a column in a `DataFrame` 

Applying the `np.square()` function to square the values in all the rows with column'Field_2' in a `Dataframe` with the method `.apply()`, similar to the concept of lambda function in modern object oriented programming.

```python
df['Field_4']=df['Field_2'].apply( np.square)
```

## Filtering values

Using the previous example you can filter values with conditional expressions. For example, filtering all the rows with `Field_1` equals to `20.0`
```python
df[df['Field_1' ] == 20.0]
```
Or the rows with `Field_2` smaller than `3.0`
```python
df[df['Field_2' ] < 3.0]
```

## Random Sampling
It is possible to take just a sample of the whole data set. In this example we take `1000` random rows
```python
df =df.sample(n=1000)
```

## Operations among columns
It is possible to do arithmetic operations with columns, for example adding `Field_1` and `Field_2` to create a new `Field_4` looks like:
```python
df['Field_5' ] = df['Field_1' ] + df['Field_2' ] 
```

Plotting with Pandas is relatively easy. The `Dataframe` object has some basic integrations with `mathplotlib`. For example a scatter plot can be plotted like:
```python
df.plot.scatter(x='Field_2', y='Field_5',s=2)
```
![](https://github.com/alvarovm/solarcelldata/blob/ff4944f70dca30ebb26d23cded7955f980ecf8f6/figures/plot1.png)

Other interesting package for visualization is `Seaborn`. Values from `Dataframes` could be extracted as list and passed directly to `Seaborn` like:
```python
import seaborn as sns
sns.distplot(df['Field_1'].values[:])
```
![](https://github.com/alvarovm/solarcelldata/blob/ff4944f70dca30ebb26d23cded7955f980ecf8f6/figures/plot2.png)

## Loading data file into a `DataFrame`

Pandas can import data a variety of formats, including  spreadsheets such as .XML and . CSV. For example, we could load a file into `Dataframe` as:

```python
df = pd.read_csv('../data/mydata.csv').fillna(value = 0)
```
Notice that in this example we are assigning a zero value to those cells that are empty.

## Storing `Dataframes`

`Dataframes` could be saved in multiple formats, such as .XML, .CSV, JSON, Pickle, etc. For example, we could save in Pickle (binary) format as:
```python
df.to_pickle('../data/mydata.pkl')
```

## Useful methods data description

In cases when the amount of data is large Pandas has methods that could give you a glimpse of the content. The method `.count()` prints out the number of elements.

```python
df.count()
```
The method `.describe()` gets a number of useful summaries for a dataset, including minimum, maximum, mean, deviation, etc.

```python
df.describe()
```

The method `.hist()` could give you a visual reference of the data, and how this is distributed.

```python
import matplotlib.pyplot as plt
df.hist(bins=50, figsize=(15,15))
plt.show()
```


# Machine Learning - Basics

## Unsupervised learning - Principal Component Analysis

Principal component analysis (PCA) is a statistical procedure that uses an orthogonal transformation to convert a set of observations of possibly correlated variables into a set of values of linearly uncorrelated variables called principal components.

In other words, PCA is a way of reducing the dimensionality of a dataset while preserving as much of the information as possible. This can be useful for visualization, as it can make it easier to see patterns in high-dimensional data. PCA can be used to reduce the dimensionality of a dataset by selecting a subset of the principal components. For example, if a dataset has 100 features, we could use PCA to reduce the dimensionality to 10 or 20 features.

PCA works by finding the directions in the data that have the most variance. These directions are called principal components. The first principal component is the direction with the most variance, the second principal component is the direction with the second most variance, and so on. 


The principal components are then ranked in order of decreasing variance. The first principal component will contain the most information about the data, the second principal component will contain the second most information, and so on.
To compute PCA, we start by standardizing the dataset, ensuring that each feature has a mean of zero and a standard deviation of one. This step is crucial for ensuring that all features contribute equally to the analysis. Next, we calculate the covariance matrix of the data, which helps us understand how the features vary together. The principal components are derived from the eigenvectors of this covariance matrix, with the corresponding eigenvalues indicating the amount of variance captured by each component.

Mathematically, if _X_ is our data matrix, the covariance matrix _C_ is computed as:

![](https://raw.githubusercontent.com/alvarovm/solarcelldata/refs/heads/main/figures/pca-eq.png)

where _n_ is the number of observations. The eigenvectors of _C_ are the principal components, and the eigenvalues indicate the variance captured by each component.

PCA is particularly useful in applications like web searching, where datasets can be vast and complex. For example, consider a search engine that processes thousands of features from web pages, such as keywords, metadata, and user interactions. By applying PCA, the search engine can reduce the dimensionality of this data, focusing on the most informative features. This simplification can enhance the efficiency of search algorithms, making it easier to identify relevant pages and improve search results.

In visualization, PCA enables us to project high-dimensional data onto a lower-dimensional space, often two or three dimensions, which can be plotted and visually inspected. This projection helps in identifying clusters, trends, and outliers, providing insights that are crucial for data-driven decision-making.

The number of principal components to select depends on the specific application. In general, we want to select enough principal components to capture as much of the information in the data as possible, while still keeping the number of dimensions manageable.

In the following example, we will operate with the `Iris` dataset, which is dataset of flowers. This dataset is included win the `sci-kit learn` package.

```python
# import packages
# datasets has the Iris dataset
from sklearn import datasets

# pandas and numPy for DataFrames and arrays
import pandas as pd
import numpy as np

# pyplot and seaborn for plots
import matplotlib.pyplot as plt
import seaborn as sns



# Get IRIS dataset
iris = datasets.load_iris()

# Load the data and target values
X, y = iris.data, iris.target

# Create DataFrame with iris data
df = pd.DataFrame(X, columns = iris.feature_names)
df2 = df.copy()
df3 = df.copy()
df.head()
```
![](https://github.com/alvarovm/solarcelldata/blob/511b48c84319019ce4149f9913e4755ebc96b4ca/figures/tnse0.png)

## Computing PCA Model

This is an example to compute a PCA and visualize the 2 most important components in a 2D plot.

```python
# Importing PCA method
from sklearn.decomposition import PCA
# Fitting
X_reduced= PCA(n_components=2).fit_transform(X)
df = pd.DataFrame(X_reduced)
# Printing results
df.columns = ['x', 'y']
df.head()
```
![](https://github.com/alvarovm/solarcelldata/blob/eff8f8992ce184f66e88fd0c373c297384a7e315/figures/pca2.png)


```python
sns.scatterplot(x='x', y='y', data=df,hue=iris.target,palette="Set1")
```

![](https://github.com/alvarovm/solarcelldata/blob/eff8f8992ce184f66e88fd0c373c297384a7e315/figures/pca1.png)


## Cluster Analysis T-SNE
t-SNE (t-distributed stochastic neighbor embedding) is a powerful non-linear dimensionality reduction algorithm that is widely used for visualizing high-dimensional data in a two or three-dimensional space. The primary goal of t-SNE is to convert similarities between data points in high-dimensional space into joint probabilities and then minimize the divergence between these joint probabilities in the high-dimensional and low-dimensional spaces. This process helps in preserving the local structure of the data, meaning that similar data points in the high-dimensional space will remain close together in the low-dimensional space, while dissimilar points will be positioned further apart.

For data visualization, t-SNE provides an effective way to explore and present high-dimensional data in a format that is accessible and interpretable. By transforming complex datasets into two or three dimensions, it enables researchers and analysts to gain insights that might be difficult to discern in the original high-dimensional space.

### Basic Computation of t-SNE

The t-SNE algorithm involves several key steps:
1. Compute Pairwise Similarities: In the high-dimensional space, the similarity between two data points is computed using a Gaussian distribution. The similarity _ pij _​ between points _xi_​ and _xj_​ is given by:

![](https://raw.githubusercontent.com/alvarovm/solarcelldata/refs/heads/main/figures/tsne1e.png)

where _σi​_ is the bandwidth of the Gaussian centered at _xi_​.

2. Symmetrize the Joint Probabilities: The joint probability _pij_​ is symmetrized as:

![](https://raw.githubusercontent.com/alvarovm/solarcelldata/refs/heads/main/figures/tsne2e.png)

3. Map to Low-Dimensional Space: In the low-dimensional space, the similarity _qij_ between points _yi_​ and _yj_​ is modeled using a Student's t-distribution with one degree of freedom (a Cauchy distribution):

![](https://raw.githubusercontent.com/alvarovm/solarcelldata/refs/heads/main/figures/tsne3e.png)

4. Minimize the Kullback-Leibler Divergence: The positions of the points in the low-dimensional space are optimized by minimizing the Kullback-Leibler divergence between the joint probabilities _pij​_ and _qij_​:

![](https://raw.githubusercontent.com/alvarovm/solarcelldata/refs/heads/main/figures/tsne4e.png)

This optimization is typically performed using gradient descent.

### How to use t-SNE with python

Similarly to the PCA example we will create a t-SNE analysis with the `Iris` dataset.

```python
# import packages
# datasets has the Iris dataset
from sklearn import datasets

# pandas and numPy for DataFrames and arrays
import pandas as pd
import numpy as np

# pyplot and seaborn for plots
import matplotlib.pyplot as plt
import seaborn as sns

# TSNE Model
from sklearn.manifold import TSNE

# Get dataset
iris = datasets.load_iris()


# load the data and target values
X, y = iris.data, iris.target

# create DataFrame with iris data
df = pd.DataFrame(X, columns = iris.feature_names)
df2 = df.copy()
df3 = df.copy()
df.head()
```

![](https://github.com/alvarovm/solarcelldata/blob/511b48c84319019ce4149f9913e4755ebc96b4ca/figures/tnse0.png)

```python
# initialize the model
model = TSNE(learning_rate=100, random_state=2)

# fit the model to the Iris Data
transformed = model.fit_transform(X)
df = pd.DataFrame(transformed)
df.columns = ['x', 'y']
df.head()
```
![](https://github.com/alvarovm/solarcelldata/blob/511b48c84319019ce4149f9913e4755ebc96b4ca/figures/tnse1.png)
```python
sns.scatterplot(x='x', y='y', data=df,hue=iris.target,palette="Set1")
```
![](https://github.com/alvarovm/solarcelldata/blob/511b48c84319019ce4149f9913e4755ebc96b4ca/figures/tnse2.png)


## Clustering with HDBSCAN
HDBSCAN (Hierarchical Density-Based Spatial Clustering of Applications with Noise) is an advanced clustering algorithm that builds upon the principles of DBSCAN, a popular density-based clustering method. Unlike traditional clustering algorithms like k-means, which partition data based on the distance between points, HDBSCAN focuses on the density of data points. This approach allows it to effectively identify clusters in datasets that are noisy or contain outliers, making it particularly useful for real-world applications where data is often imperfect.

HDBSCAN is a density-based algorithm, which means that it only considers the density of data points when clustering. This makes it different from other clustering algorithms, such as k-means, which consider the distance between data points when clustering.

HDBSCAN is a relatively new algorithm, but it has been shown to be effective in a variety of clustering tasks. It is particularly well-suited for clustering data that is not well-separated, such as data that has noise or outliers.

### How HDBSCAN Works

1. Density-Based Clustering: HDBSCAN starts by identifying regions of high density in the data. It defines density using the concept of "core distance," which is the distance to the nearest point that satisfies a minimum number of neighbors. This is crucial for understanding the local density around each point.

2. Hierarchical Clustering: Once dense regions are identified, HDBSCAN constructs a hierarchy of clusters by progressively merging these regions based on their density connectivity. This hierarchical approach allows it to capture the natural structure of the data at different levels of granularity.

3. Extraction of Stable Clusters: From the hierarchy, HDBSCAN extracts the most stable clusters using a measure called "excess of mass." This ensures that the resulting clusters are robust and meaningful, even in the presence of noise.

Elemental Equations

While HDBSCAN involves complex computations, some fundamental concepts can be simplified for understanding:

* Core Distance: For a point pp, the core distance core(p)core(p) is defined as the distance to its kk-th nearest neighbor, where kk is a parameter specifying the minimum number of points required to form a dense region.
    _core(p)=distance(p,k-th nearest neighbor)_


*Mutual Reachability Distance: Between two points pp and qq, the mutual reachability distance is defined as:
    _mreach(p,q)=max⁡(core(p),core(q),distance(p,q))_
 
This distance metric helps in constructing the hierarchy by considering both the density and the spatial proximity of points

#### Using `hdbscan` module in python

Using the `Dataframe` from the t-SNE analysis in the previous section, we could obtain the clusters from HDSCAN with the following script:

```python

import hdbscan
cluster_tsne = hdbscan.HDBSCAN(min_cluster_size=2
                               , gen_min_span_tree=True)
cluster_tsne.fit(transformed)
plt.figure(figsize=(6,8))
plt.scatter(transformed.T[0], transformed.T[1], marker='o',c=cluster_tsne.labels_,s=5, cmap='brg')
#plt.scatter(tsne_X.T[0], df[ 'lambda_sTDA (nm)'][x_index].values[:]  , marker='o',c=cluster_tsne.labels_,s=50, cmap='hsv')
        

#plt.scatter(X_skernpca[y==1, 0], X_skernpca[y==1, 1], 
#             color='blue', marker='o', alpha=0.5)

plt.xlabel('X')
plt.ylabel('Y')
plt.title('Organic Dyes: HDBSCAN')
cbar = plt.colorbar(orientation='horizontal')
cbar.set_label('Cluster Label')
plt.show()
```
![](https://github.com/alvarovm/solarcelldata/blob/a699855ae52d868a3631a4be54fb028bd5d44509/figures/hdbscan.png)

Alternatively, we could append the HDSCAN results to our `Dataframe`. For example:
 
```python
df['cluster']=cluster_tsne.labels_
df['clusterprob']=cluster_tsne.probabilities_
```

## Supervised learning - Gaussian Regression Process

Gaussian process regression (GPR) is a machine learning technique that can be used to predict a continuous value (such as temperature or height) from a set of input data. Unlike parametric methods, which assume a specific form for the data distribution, GPR is nonparametric.  This means it does not impose a predefined structure on the data, making it highly adaptable to various types of datasets.

GPR works by assuming that the function that maps from the input data to the output value is a Gaussian process. A Gaussian process is a distribution over functions that means that any finite number of outputs from the function will be jointly Gaussian distributed. This means that we can use the properties of Gaussian distributions to make predictions about the output value for a new input.

One of the advantages of GPR is that it can provide uncertainty estimates for its predictions. This is because the Gaussian process distribution also defines a distribution over the uncertainty of the predictions. This can be useful for applications where it is important to know how confident we are in our predictions.

GPR is a powerful machine learning technique that can be used for a variety of tasks. It is particularly well-suited for tasks where the underlying distribution of the data is unknown or where it is important to be able to provide uncertainty estimates for the predictions.

[See more about Gaussian Process in wikipedia](https://en.wikipedia.org/wiki/Gaussian_process)
[Learn more, visual exploration](https://distill.pub/2019/visual-exploration-gaussian-processes/)


```python
import numpy as np
import pandas as pd
from sklearn.gaussian_process import GaussianProcessRegressor
from sklearn.datasets import load_iris

# Load the iris dataset
iris = load_iris()

# Create the features and target variables
X = iris.data
y = iris.target

# Create a Gaussian process regressor
gpr = GaussianProcessRegressor()

# Fit the regressor to the data
gpr.fit(X, y)

# Predict the target values for the test data
predictions = gpr.predict(X)

# Calculate the mean squared error
mse = np.mean((predictions - y)**2)

print("Mean squared error:", mse)
```
`Mean squared error: 0.006245333333333333`

## Supervised learning - Random Forest
Random Forest is a popular and powerful machine learning method used primarily for classification and regression tasks. It is an ensemble learning technique, which means it builds multiple models and combines them to improve overall performance. Specifically, Random Forest constructs a multitude of decision trees during training and outputs the mode of their classes (for classification) or the mean prediction (for regression) as the final result.

To understand how random forest works, let's first take a look at decision trees. A decision tree is a simple machine learning algorithm that can be used to make predictions by splitting the data into smaller and smaller groups until the predictions are clear. For example, a decision tree could be used to predict whether a patient has cancer by asking a series of questions about the patient's symptoms.

Random forest works by creating a set of decision trees, each of which is trained on a different subset of the data. The trees are created by randomly selecting features from the data and then splitting the data based on those features. This process is repeated until the desired number of trees is created. This means some data points may be repeated in a sample, while others may be left out.

In addition to sampling data points, Random Forest introduces randomness by selecting a random subset of features for each split in a tree. This helps in making the trees less correlated and more diverse, which improves the robustness of the model.

The final prediction is made by combining the predictions of the individual trees. This is done by taking the majority vote of the trees for classification tasks or the average of the trees for regression tasks.

```python
import numpy as np
import pandas as pd
from sklearn.ensemble import RandomForestRegressor
from sklearn.datasets import load_iris

# Load the iris dataset
iris = load_iris()

# Create the features and target variables
X = iris.data
y = iris.target

# Create a random forest regressor with 100 trees
rf = RandomForestRegressor(n_estimators=100)

# Fit the regressor to the data
rf.fit(X, y)

# Predict the target values for the test data
predictions = rf.predict(X)

# Calculate the mean squared error
mse = np.mean((predictions - y)**2)

print("Mean squared error:", mse)
```
`Mean squared error: 0.006245333333333333`

## Feature selection with Random Forest method
Feature selection is the process of selecting the most important features from a dataset for a machine learning model. This can be done for a variety of reasons, such as to improve the accuracy of the model, to reduce the complexity of the model, or to make the model more interpretable.

Random forest is a machine learning algorithm that can be used for both classification and regression tasks. It is a type of ensemble learning algorithm, which means that it combines the predictions of multiple decision trees to make a final prediction.

Random forest can also be used for feature selection. The way it works is that each decision tree in the random forest is trained on a different subset of the features. This means that each tree will learn to focus on different features. The importance of a feature is then determined by how often it is used to split the data in the decision trees.

The features with the highest importance are the ones that are most important for making predictions. These features can then be selected for the machine learning model.

```python
import numpy as np
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_iris

# Load the iris dataset
iris = load_iris()

# Create the features and target variables
X = iris.data
y = iris.target

# Create a random forest classifier
rf = RandomForestClassifier(n_estimators=100)

# Fit the classifier to the data
rf.fit(X, y)

# Calculate the feature importances
importances = rf.feature_importances_

# Print the feature importances
print(importances)

# Select the top 2 features
top_2_features = np.argsort(importances)[-2:]

# Print the top 2 features
print(top_2_features)
```

```
[0.09179974 0.02709257 0.45509135 0.42601634]
[3 2]
```

# RDKIT

RDKit is an open-source software toolkit designed for cheminformatics, which is the application of computer science to chemical data. It provides a collection of tools for handling and analyzing chemical information, making it essential for tasks like molecular modeling, drug discovery, and chemical database management. RDKit is particularly popular in computational chemistry and bioinformatics due to its versatility and integration capabilities.

Key features of RDKit include the ability to read and write various chemical file formats, perform substructure searches, generate molecular fingerprints, and calculate molecular descriptors. It also supports molecular visualization and offers functionalities for simulating chemical reactions. RDKit is implemented in C++ but provides Python bindings, making it accessible and easy to use for those familiar with Python programming.

## Converting `SMILES`  to a RDKIT molecule object
SMILES (Simplified Molecular Input Line Entry System) strings are a notation system used to represent chemical structures in a linear text format. They encode molecular structures using short ASCII strings, allowing for easy storage and manipulation in computational applications. SMILES strings capture the connectivity and arrangement of atoms, bonds, and rings in a molecule. For example, "C" represents a carbon atom, while "O" represents oxygen. SMILES are widely used in cheminformatics for tasks like database searching and molecular modeling, providing a compact and efficient way to handle chemical information in software applications.

```python
from rdkit.Chem import MolFromSmiles as smi2mol
#SMILES strings example
smi = 'Clc1ccc(c(c1)Cl)OCCn1cncc1'
mol = smi2mol(smi)
#Print out molecule plot
Draw.MolToImage(mol,size=(800, 800))
```
![](https://github.com/alvarovm/solarcelldata/blob/a0a9c89f1b73656128c602476ec7413c22bd39ec/figures/smi2mol.png)

## Visualizing data in a grid

```python
from rdkit.Chem import Draw
smiles = ['N#CC(=Cc1ccc(N(c2ccccc2)c2ccccc2)cc1)c1ccc(Cl)cc1','N#CC(=Cc1ccc(N(c2ccccc2)c2ccccc2)cc1)c1ccc(Br)cc1']
lengend=['Cl','Br']
from rdkit.Chem import MolFromSmiles as smi2mol
mol = [smi2mol(e) for e in smiles]
Draw.MolsToGridImage(mol, molsPerRow=2, subImgSize=(250,250), legends=['X = {}'.format(x) for x in lengend] )
```
![](https://github.com/alvarovm/solarcelldata/blob/5add624a72bae9b70a17ea016de2e50c9d08d2fa/figures/molgrid.png)

## Computing the fingerprint of a molecule

One way to translate molecules in forms that computers could represent as mathematical objects, such as vector or matrices, are the fingerprints. In this representation we could compare molecules and define similarity. Rdkit package include a good number of fingerprints, see this
[tutorial](https://greglandrum.github.io/rdkit-blog/posts/2023-01-18-fingerprint-generator-tutorial.html).

For example, the Morgan fingerprint constructs vectors fragmenting molecules around an atom  within a radius. The algorithm counts how many fragments are present and produces a vector with positive real numbers. Alternatively, this could produce a bit vectors that just indicate whether the fragment is present or not. For small and medium size molecules radius 6 and vector length of 1024 is enough. See the follow example for a bit vector.

```python
from rdkit.Chem.rdMolDescriptors import  GetMorganFingerprintAsBitVect
np.array(GetMorganFingerprintAsBitVect(mol, radius=6, nBits=2048))
```

`array([0, 0, 0, ..., 0, 0, 1])`

Visual representation of the Morgan Fingerprint algorithm:
![Morgan FP](https://blog.dnanexus.com/hs-fs/hubfs/Imported_Blog_Media/Morgan-Algorithm-1024x653.png?width=512&height=323&name=Morgan-Algorithm-1024x653.png)

## Computing molecular descriptors
A molecular descriptor in cheminformatics is a numerical value that summarizes a particular property of a molecule. Many times descriptors are not unique, therefore, for machine learning is preferable to use a set of descriptors. For an overview of the descriptor implemented in RDkit visit [this blog](https://greglandrum.github.io/rdkit-blog/posts/2022-12-23-descriptor-tutorial.html).
```python
import rdkit.Chem.Descriptors as Descriptors
#Compute molecular weight
Descriptors.MolWt(mol)
```

`257.11999999999995`

# Getting help from Large Language Models (LLM)

LLM are becoming a very helpful tool for coding. Indeed, it estimated that a large portion of commercial codes are AI generated. 30% of Microsoft is presumable AI generated.

You can get help to complete your project using your favorite LLM (openai.com, anthropic.com, mistral.ai, cohere.com, deepseek.com, duck.ai, google.com/gemini, etc). The following example was tested with the free service [www.duck.ai](https://www.duck.ai/).
Try:
```
You are a helpful computer science assistant and expert in python. 
Provide a python class to compute Fibonacci numbers and returns 
a numpy array for the first 10 Fibonacci numbers.
```
Notice that in this example we define a role ("You are a helpful computer science assistant and expert in python"). And our request is very specific of the language, algorithm and libraries used.

The response will be:

```python
import numpy as np

class Fibonacci:
    def __init__(self, n):
        self.n = n
        self.fib_sequence = self.compute_fibonacci()

    def compute_fibonacci(self):
        fib = [0, 1]
        for i in range(2, self.n):
            next_fib = fib[i - 1] + fib[i - 2]
            fib.append(next_fib)
        return np.array(fib)

    def get_fibonacci_array(self):
        return self.fib_sequence

# Create an instance of the Fibonacci class for the first 10 Fibonacci numbers
fibonacci_instance = Fibonacci(10)
fibonacci_array = fibonacci_instance.get_fibonacci_array()

print(fibonacci_array)
```

Also you can use LLMs to edit and explain your code, this could be done adding your code to the prompt.

Be aware that the code generated by the LLM could be incorrect, and in some cases could be harmful. Also remember that most of the *free* services could record your data, so be careful with including personal, proprietary, or export controlled data in your prompts. You can learn more about prompting in [this video](https://www.youtube.com/watch?v=ysPbXH0LpIE) from Antropic AI.



