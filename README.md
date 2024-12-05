# Fashion Mnist Clusturing

Fashion-MNIST is a dataset of Zalando's article images: <https://github.com/zalandoresearch/fashion-mnist>  

Zalando intends Fashion-MNIST to serve as a direct drop-in replacement for the original MNIST dataset for benchmarking machine learning algorithms. It shares the same image size and structure of training and testing splits.

## Main Objective & Dataset Characteristics

My objective is to explore the performance of diverse clustering algorithms on the Fashion MNIST dataset from Zalando. As part of this process, I plan to apply dimensionality reduction techniques to better understand the underlying structure of the data and optimize clustering performance. My goal is to achieve coherent clustering, meaning that items within the same cluster should share visually similar characteristics (e.g., style, type of clothing, or purpose).  

Although the dataset is labeled, I will use the labels only for validation purposes, comparing the clustering results to the ground truth to assess coherence and accuracy.

**Main Caracteristics**:

- Fashion-MNIST is a dataset of Zalando's article images, consisting of a training set of 60,000 examples and a test set of 10,000 examples.
- It was intended to serve as a direct drop-in replacement for the original MNIST dataset (handwriten digits) for benchmarking machine learning algorithms.
- Each example is a 28x28 grayscale image, ie 784 pixel values in total, higher value meaning darker pixel. This pixel-value is an integer between 0 and 255.
- To locate a pixel on the image, suppose that we have decomposed x as x = i * 28 + j, where i and j are integers between 0 and 27. The pixel is located on row i and column j of a 28 x 28 matrix.
- For example, pixel31 indicates the pixel that is in the fourth column from the left, and the second row from the top.

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

## Data Analysis Findings

- The pixels in the center of the images are on average much darker and vary less than those in the inner periphery.
- We notice that pixels on the outer periphery are almost never activated, and that the rate of activated pixels increases drastically as we approach the center of the image.
- The distributions of the values taken for the center and inner periphery pixels are not too skewed, whereas those of the outer periphery are much more skewed, which could have a negative impact on the quality of predictions, log(1+skew) in [-2, 0.5] means skew in [-0.8647, 0.64872].
- Less than 50% of the pixels are activated across the dataset.
  - It could have negative impact on predictions and algorithms performances.
  - It could be worth to experiment with cosine similarity to mitigate this result.
- 2D exploration
  - Techniques such as PCA or NMF struggle to display groups in 2D
  - TSNE achieve a good grouping in 2D but takes ages to run (I ve sample the data to run it on only 10% of the dataset size).
  - UMAP achieve a very good space splitting and run on acceptable time on the full dataset.
- 3D exploration
  - PCA seems to still struggle to split the data in groups
  - NMF achieve some better results defining regions of spaces where some classes prevail on other but still no clean cuts in the data
  - T-SNE have good results in 3D but still takes too long to run
  - UMAP achieve great results grouping the data in 4 distincts parts in decsent time
- 2D and 3D dimensionality reduction with cosine distance
  - PCA and NMF do not have the possibility to apply cosine distance
  - For T-SNE it seems to allow for better grouping distinction but still take ages to run.
  - For UMAP, it seems to define the sames groups than with euclidian distance, therefore it does not add so much value to use cosine instead of euclidian.
