# Fashion Mnist Clusturing

Fashion-MNIST is a dataset of Zalando's article images: <https://github.com/zalandoresearch/fashion-mnist>  

Zalando intends Fashion-MNIST to serve as a direct drop-in replacement for the original MNIST dataset (handwriten digits) for benchmarking machine learning algorithms.

## Main Objective

The goal of this project is to evaluate the effectiveness of various clustering techniques on the Fashion-MNIST dataset. This involves reducing the dimensionality of the dataset using techniques like PCA, NMF, and UMAP to improve clustering outcomes. The analysis focuses on achieving meaningful clusters where items within the same cluster share similar visual attributes, like clothing type or style.

Although the dataset is labeled, I will use the labels only for validation purposes, comparing the clustering results to the ground truth to assess coherence and accuracy.

## Dataset Characteristics

### Dataset Overview

- Fashion-MNIST is consisting of a training set of 60,000 examples and a test set of 10,000 examples.
- Each example is a 28x28 grayscale image, ie 784 pixel values in total, higher value meaning darker pixel. This pixel-value is an integer between 0 and 255.
- Each image is associated with a label from 10 classes, envenly distributed (6000 each) :

| Label | Description |
| --- | --- |
| 0 | T-shirt/top |
| 1 | Trouser |
| 2 | Pullover |
| 3 | Dress |
| 4 | Coat |
| 5 | Sandal |
| 6 | Shirt |
| 7 | Sneaker |
| 8 | Bag |
| 9 | Ankle boot |

### Analysis Findings

- Pixels in the center of the images are on average darker, vary less and are more often activated than pixels in the periphery.
- Techniques like PCA and NMF struggled to display meaningful groupings in 2D and 3D visualizations.
- [UMAP](https://umap-learn.readthedocs.io/en/latest/index.html) provides the most coherent groupings in 2D and 3D visualizations.

## Unsupervised Learning Models & Selection

Several unsupervised learning models were applied to the Fashion MNIST dataset, including:

- **K-Means**: A widely-used clustering algorithm that partitions the data into K clusters based on the mean of each cluster.
- **Agglomerative Clustering**: A hierarchical clustering method that groups data points based on a distance metric, iteratively merging clusters.
- **HDBSCAN** (Hierarchical Density-Based Spatial Clustering of Applications with Noise): an extension of DBSCAN that finds clusters of varying density and combines the results to improve stability.
- **Dimensionality Reduction Techniques** (PCA, NMF, UMAP): These techniques were applied to reduce the high-dimensional dataset to a lower-dimensional space that is more suitable for clustering.

To evaluate the models, I used the *Adjusted Rand Index* and *Adjusted Mutual Information*, two robust metrics for assessing clustering performance when ground truth labels are available.

### Best Model for Main Objective

From the clustering experiments, Ward Agglomerative Clustering with UMAP-transformed data, while not perfect, provided the best results. This model achieved coherent clusters that were both computationally efficient and aligned well with the ground truth categories.

## Model Flaws & Plan for Improvement

**Model Limitations**:

- Clustering performance varies depending on the dimensionality reduction method. Algorithms seem to performed well with UMAP, but not with PCA or NMF.
- Large memory requirements, agglomerative clustering takes huge RAM on the full dataset.
- Reconstruction of original data is difficult with non-linear methods like UMAP, which prioritize local geometry preservation over global structure.

**Plan for Revisiting the Analysis**:

- In depth Fine tuning of models, especially HDBSCAN, to better match the ground truth clusters.
- Exploring Alternative Clustering Methods: Testing more clustering algorithms, such as OPTICS or Gaussian Mixture Models, could provide better segment.
- Alternative dimention reduction approaches which could involve using the dense mapper from the UMAP package, which better preserves local density, or utilizing a mutual k-NN graph. These methods could enhance the segmentation of similar classes, allowing clustering to generate more accurate results and potentially increase the number of clusters produced by HDBSCAN to align more closely with the ground truth.
- Explore Deep Learning solutions (generally better suited to image analysis)

## Annexe: Detailed Scoring

| model   | dimension reduction | n components | adjusted rand index | adjusted mutual information |
| ------- | ------------------- | ------------ | ------------------- | --------------------------- |
| ward    | umap                |           10 |           0.472368  |                    0.656989 |
| k-means | umap                |           20 |           0.454139  |                    0.633707 |
| k-means | umap                |           80 |           0.454360  |                    0.633524 |
| ward    | umap                |           20 |           0.471000  |                    0.630971 |
| ward    | umap                |           80 |           0.433626  |                    0.622254 |
| k-means | umap                |           10 |           0.456125  |                    0.622112 |
| hdbscan | umap                |           10 |           0.297293  |                    0.615377 |
| hdbscan | umap                |           20 |           0.297362  |                    0.615279 |
| hdbscan | umap                |           80 |           0.297342  |                    0.614638 |
| ward    | NaN                 |          NaN |           0.378539  |                    0.587801 |  
| k-means | NaN                 |          NaN |           0.404701  |                    0.542046 |  
| ward    | pca                 |           20 |           0.363729  |                    0.537645 |
| ward    | pca                 |           10 |           0.346739  |                    0.525471 |
| k-means | pca                 |           80 |           0.390737  |                    0.523687 |
| ward    | pca                 |           80 |           0.351579  |                    0.516891 |
| k-means | pca                 |           20 |           0.347294  |                    0.507024 |
| k-means | pca                 |           10 |           0.338712  |                    0.488058 |
| ward    | nmf                 |           10 |           0.243654  |                    0.440881 |
| k-means | nmf                 |           10 |           0.232076  |                    0.434490 |
| ward    | nmf                 |           20 |           0.268024  |                    0.410812 |
| k-means | nmf                 |           20 |           0.256232  |                    0.406255 |
| ward    | nmf                 |           80 |           0.222787  |                    0.375820 |
| k-means | nmf                 |           80 |           0.209876  |                    0.363655 |
| hdbscan | nmf                 |           10 |           0.033482  |                    0.299475 |
| hdbscan | nmf                 |           20 |           0.034714  |                    0.274219 |
| hdbscan | nmf                 |           80 |           0.077794  |                    0.214393 |
| hdbscan | NaN                 |          NaN |           0.043650  |                    0.113009 |  
| hdbscan | pca                 |           10 |           0.017095  |                    0.092629 |
| hdbscan | pca                 |           20 |           0.020170  |                    0.085466 |
| hdbscan | pca                 |           80 |           0.018948  |                    0.081460 |
