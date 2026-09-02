# DeepMPRA

Microplastic risk assessment method

## Problem addressed

Current ecological risk assessment of microplastics in lakes mainly relies on linear index systems. These systems require manually set background values and assume that risk factors, such as abundance, particle size, and polymer type, interact in a purely additive and linear manner. This linear assumption ignores the complex nonlinear interactions among factors and can easily lead to an unreasonable representation of the actual risk pattern. DeepMPRA learns risk thresholds entirely from data, without preset background concentrations, and uses nonlinear modeling to capture the true risk structure, thereby effectively correcting the assessment bias inherent in conventional methods.

## Highlights

* **No thresholds and no background values**

Fully data driven, with no need to manually define risk level boundaries or set preset background concentrations.

* **Nonlinear risk modeling**

Uses an autoencoder to extract nonlinear interactions among microplastic features. Based on unsupervised clustering, it defines relative risk according to how far a sample deviates from a low risk reference state, rather than calculating an absolute risk index.

## Usage instructions

### Data requirements

The dataset must contain the columns `abundance` and `MDII`.

The categorical attributes of microplastics (shape, size, type, and color) do not need to be limited to the categories listed in the example dataset (such as fragment, film, fiber, pellet, foam, and other). You can classify these attributes according to your own primary classification scheme. Any category that does not belong to the primary classification should be placed in the "other" category. For each sample, the proportions of all categories within a given attribute must sum to 100%. This principle also applies to size, type, and color.

The dataset provided in this repository was imputed using only the mean values reported in the article. If a more complete dataset is needed, you can use missForest for multiple imputation, or you can use other imputation methods to fill missing values.

### Core function descriptions

1. `dataset_split.ipynb` splits the dataset into training and test sets to prevent data leakage in downstream microplastic risk analyses.

2. `data_augmentation.ipynb` uses GAN to augment the data. Note: after augmentation, MDII must be recalculated using the original formula. MDII values generated directly by the generative adversarial network (GAN) must not be used.

3. `microplastic_risk_assessment.ipynb` contains the microplastic risk assessment pipeline. It includes training an autoencoder and extracting latent representations of samples, using principal component analysis (PCA) to determine the direction of the risk attribute and calculating the projection of each sample onto that direction, preprocessing and normalizing abundance and microplastic risk features, using K means clustering to identify healthy (low risk) sample clusters, and fitting a Gaussian mixture model (GMM) to the low risk samples and calculating the weighted distance and log probability for each sample.
