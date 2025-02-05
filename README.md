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

## Unsupervised Learning Models

Several unsupervised learning models were applied to the Fashion MNIST dataset, including:

- **K-Means**: A widely-used clustering algorithm that partitions the data into K clusters based on the mean of each cluster.
- **Agglomerative Clustering**: A hierarchical clustering method that groups data points based on a distance metric, iteratively merging clusters.
- **HDBSCAN** (Hierarchical Density-Based Spatial Clustering of Applications with Noise): an extension of DBSCAN that finds clusters of varying density and combines the results to improve stability.
- **Dimensionality Reduction Techniques** (PCA, NMF, UMAP): These techniques were applied to reduce the high-dimensional dataset to a lower-dimensional space that is more suitable for clustering.
- **Autoencoders** and **Convolutional Autoencoders**: These deep learning models were used to learn compact representations of the Fashion-MNIST dataset, which were then used for clustering. The autoencoders were compared to traditional methods (PCA, NMF, UMAP) in terms of how well their embeddings performed in clustering tasks.

By incorporating autoencoders and convolutional autoencoders, I aimed to explore how deep learning methods can provide better insights into complex datasets compared to traditional dimensionality reduction methods.

To evaluate the models, I used the *Adjusted Rand Index* and *Adjusted Mutual Information*, two robust metrics for assessing clustering performance when ground truth labels are available.

### Best Model for Main Objective

From the clustering experiments, Ward Agglomerative Clustering with UMAP-transformed data, while not perfect, provided the best results. This model achieved coherent clusters that were both computationally efficient and aligned well with the ground truth categories.

## Model Flaws & Plan for Improvement

**Model Limitations**:

- Clustering performance varies depending on the dimensionality reduction method. Algorithms seem to performed well with UMAP, but not with PCA or NMF.
- Large memory requirements, agglomerative clustering takes huge RAM on the full dataset.
- Reconstruction of original data is difficult with non-linear methods like UMAP, which prioritize local geometry preservation over global structure.
- When leveraging deep learning models, we often trade explainability for improved performance.

**Plan for Revisiting the Analysis**:

- In depth Fine tuning of models, especially HDBSCAN, to better match the ground truth clusters.
- Exploring Alternative Clustering Methods: Testing more clustering algorithms, such as OPTICS or Gaussian Mixture Models, could provide better segment.
- Alternative dimention reduction approaches which could involve using the dense mapper from the UMAP package, which better preserves local density, or utilizing a mutual k-NN graph. These methods could enhance the segmentation of similar classes, allowing clustering to generate more accurate results and potentially increase the number of clusters produced by HDBSCAN to align more closely with the ground truth.
- Approaches leveraging autoencoders, particularly the convolutional autoencoder, could be further improved with more advanced and complex network architectures.

## Annexe: Detailed Scoring

| model   | dimension reduction   |   n components |   adjusted rand index |  adjusted mutual information |
|---------|-----------------------|----------------|-----------------------|------------------------------|
| ward    | umap                  |             10 |            0.472368   |                    0.656989  |
| ward    | umap                  |             20 |            0.471      |                    0.630971  |
| k-means | umap                  |             10 |            0.456125   |                    0.622112  |
| k-means | umap                  |             80 |            0.45436    |                    0.633524  |
| k-means | umap                  |             20 |            0.454139   |                    0.633707  |
| ward    | umap                  |             80 |            0.433626   |                    0.622254  |
| ward    | cae                   |             10 |            0.4266     |                    0.592642  |
| ward    | cae                   |             20 |            0.420423   |                    0.605746  |
| k-means | nan                   |            nan |            0.404701   |                    0.542046  |
| k-means | cae                   |             80 |            0.395398   |                    0.57013   |
| k-means | pca                   |             80 |            0.390737   |                    0.523687  |
| k-means | cae                   |             10 |            0.386969   |                    0.562585  |
| ward    | ae                    |             80 |            0.380241   |                    0.545681  |
| ward    | nan                   |            nan |            0.378539   |                    0.587801  |
| ward    | cae                   |             80 |            0.373973   |                    0.5708    |
| k-means | cae                   |             20 |            0.3645     |                    0.554621  |
| ward    | pca                   |             20 |            0.363729   |                    0.537645  |
| ward    | pca                   |             80 |            0.351579   |                    0.516891  |
| k-means | pca                   |             20 |            0.347294   |                    0.507024  |
| ward    | pca                   |             10 |            0.346739   |                    0.525471  |
| k-means | pca                   |             10 |            0.338712   |                    0.488058  |
| ward    | ae                    |             20 |            0.326728   |                    0.505584  |
| k-means | ae                    |             10 |            0.322634   |                    0.480481  |
| k-means | ae                    |             80 |            0.311023   |                    0.493711  |
| ward    | ae                    |             10 |            0.298334   |                    0.499861  |
| hdbscan | umap                  |             20 |            0.297362   |                    0.615279  |
| hdbscan | umap                  |             80 |            0.297342   |                    0.614638  |
| hdbscan | umap                  |             10 |            0.297293   |                    0.615377  |
| k-means | ae                    |             20 |            0.281184   |                    0.469104  |
| ward    | nmf                   |             20 |            0.268024   |                    0.410812  |
| k-means | nmf                   |             20 |            0.256232   |                    0.406255  |
| ward    | nmf                   |             10 |            0.243654   |                    0.440881  |
| k-means | nmf                   |             10 |            0.232076   |                    0.43449   |
| ward    | nmf                   |             80 |            0.222787   |                    0.37582   |
| k-means | nmf                   |             80 |            0.209876   |                    0.363655  |
| hdbscan | cae                   |             10 |            0.163063   |                    0.399031  |
| hdbscan | cae                   |             80 |            0.15188    |                    0.326022  |
| hdbscan | cae                   |             20 |            0.151005   |                    0.327555  |
| hdbscan | nmf                   |             80 |            0.077794   |                    0.214393  |
| hdbscan | ae                    |             10 |            0.0463159  |                    0.304774  |
| hdbscan | nan                   |            nan |            0.0436502  |                    0.113009  |
| hdbscan | nmf                   |             20 |            0.0347136  |                    0.274219  |
| hdbscan | nmf                   |             10 |            0.0334821  |                    0.299475  |
| hdbscan | ae                    |             80 |            0.0241169  |                    0.0566632 |
| hdbscan | pca                   |             20 |            0.0201702  |                    0.0854661 |
| hdbscan | pca                   |             80 |            0.0189481  |                    0.0814599 |
| hdbscan | pca                   |             10 |            0.0170953  |                    0.0926293 |
| hdbscan | ae                    |             20 |            0.00506183 |                    0.0209042 |
