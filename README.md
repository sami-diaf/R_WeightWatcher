# R_WeightWatcher


R code to run WeightWatcher, an assessment toolkit for Deep Learning models, using reticulate and several Python libraries.

The goal is to evaluate the godnees of fit of neural networks based on the statistical properties of their weight matrices. [`Paper`](https://arxiv.org/abs/2507.17912)

Elements of Random Matrix Theory (RMT) are applied to study the spectrum of eigenvalues, within each layer and derive accuracy measurements.

WeightWatcher could be applied to single weight matrices of a given system or model (example below).

[`WeightWatcher Github reposity`](https://github.com/CalculatedContent/WeightWatcher)


## Installation

Install reticulate and link it to a python environment

```R
install.packages("reticulate")
library("reticulate)
use_virtualenv("..your_environment")
```


## Install necessary packages

In addition to the WeightWatcher Python package, several other packages are needed (numpy, torch, ...) depending on your application.

Numpy and Sklearn are needed to tailor your R output to Python standards.

```R
py_install(c("weightwatcher","torch","torch.nn","numpy","sklearn))
```

## Usage


```R
# Load python libraries in R
ww <- import("weightwatcher")
torch <- import("torch")
nn <- import("torch.nn")
#np <- import("numpy")
#sklearn <- import("sklearn")

```

For a demonstration, We will initialize a 500x500 matrix with elements from a normal distribution with mean 0 and variance 1 .

```R
# Initialize the model
input_size = 500L  # Example input size
output_size = 500L  # Example output size
model = nn$Linear(input_size, output_size, bias=FALSE)

# Set custom weights
weight_matrix = torch$randn(500L,500L)
model$weight = nn$Parameter(weight_matrix)

watcher = ww$WeightWatcher()
output = watcher$analyze(model=model)

# Visualize statistics
View(output)

# Getting additional plots (Empirical Spectral Density, Kullback-Leibler distance,...)
output_plot = watcher$analyze(model=model, plot=TRUE)
```

