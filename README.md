# DeepMPRA
Method for Risk Assessment of Microplastics

**Problem addressed**

Current ecological risk assessment of microplastics in lakes relies primarily on linear index systems, which require manually defined background values and assume that risk factors—such as abundance, particle size, and polymer type—combine in a purely additive, linear manner. This linear assumption neglects the complex nonlinear interactions among these factors and can easily lead to unreasonable characterizations of true risk patterns. DeepMPRA learns risk thresholds entirely from data without the need for preset background values, captures the authentic risk structure through nonlinear modeling, and thereby effectively corrects the assessment biases inherent in conventional approaches.

**Highlights**

- **Threshold-free and background-value-free**  
  Entirely data-driven, requiring no manual delineation of risk-level boundaries or preset background concentrations.

- **Nonlinear risk modeling**  
  Employs an autoencoder to extract nonlinear interactions among microplastic features and, based on unsupervised clustering, defines relative risk according to the degree to which a sample deviates from a “low-risk reference state,” rather than computing an absolute risk index.

**Usage notes**

*Data requirements*  
The dataset must contain the columns `abundance` and `MDII`.  
The categorical attributes of microplastics (shape, size, type, color) need not be restricted to the categories present in the example dataset (e.g., fragment, film, fiber, pellet, Foam, Others). You may classify them according to your own primary classification scheme. Any categories that do not belong to the main classification should be grouped into “Others,” and it must be ensured that, for each sample point, the proportions of all categories within a given attribute sum to 100%. The same principle applies to size, type, and color.

*Core function descriptions*  

- **`GANAugmentor`** – Performs GAN-based data augmentation on categorical features of microplastics such as shape, size, type, and color. Note: After augmentation, MDII must be recalculated using the original formula; the MDII values generated directly by the GAN must not be used.  
- **`ContinuousGANAugmentor`** – Performs GAN-based data augmentation on microplastic abundance.  
- **`load_and_prepare_data`** – Loads data and constructs the feature matrix (does not perform training/test splitting).  
- **`train_autoencoder`** – Trains the autoencoder and extracts latent representations of the samples.  
- **`compute_risk_direction`** – Uses PCA to determine the direction of the risk attribute and computes the projection of each sample onto that direction.  
- **`process_abundance_and_scale`** – Preprocesses and scales abundance and microplastic risk features.  
- **`identify_healthy_cluster`** – Uses K‑means clustering to identify healthy (low‑risk) sample clusters.  
- **`fit_gmm_and_compute_distances`** – Fits a Gaussian Mixture Model (GMM) to the low‑risk samples and computes weighted distances and log‑probabilities for every sample.  
- **`assign_risk_levels`** – Assigns microplastic risk levels to samples based on the computed distances and probabilities.  
- **`build_result_dataframe`** – Constructs a DataFrame containing the final assessment results.  
- **`predict_test_set`** – Applies the trained model to perform risk assessment on an independent test set.  

**Important:** Before using this assessment framework, you must partition your data into training and test sets yourself; `load_and_prepare_data` does not perform this split automatically.
