K-Means---FPGA-Zynq-7000-Project:
Two different k-means versions of VHDL HDL on a Cora Zynq board.

Sumnmary:

The point of this project was to compare two different implementations of the K-Means
algorithm (described below this paragraph) where one implementation would be done in
software and the second would be done in hardware on an FPGA using VHDL. The FPGA board
used was a Cora Z7 with a Zynq 7000 SoC. Two different hardware implementations are
provided for download. One being where the data was moved in partial chunks between the
ARM and the FPGA and other being where all the data was to be moved a single time into and
out of the hardware. The single data movement design was never implemented and tested in
hardware due to BRAM constraints.
It was found that the way I was exchanging data between the ARM processor and the FPGA took
up nearly all of the total FPGA runtime. For example, it took ~13 [ms] to ingress the data and
~18 ms to egress the data out of the FPGA while the hardware compute time was ~986 [us]. A
DMA controller should have been implemented moving the total amount of data in and out
each in a single step rather than multiple iterations. Overall considering the total runtime of
each implementation there was ~70 [ms] for the C software implementation and ~50 [ms] for
the FPGA solution netting a total ~(70/50) = 1.4 speedup. Other techniques that should have
been explored were adding FIFOs and pipelining in the hardware along with the ingress/egress
of the total data mentioned above.
K-means is a popular unsupervised machine learning algorithm used for clustering data points
into groups or clusters based on their similarity. It follows these main steps:
1. Initialization: Choose the number of clusters (K) that you want to form and randomly
initialize the centroids (K points) in the feature space. These centroids will serve as the
initial cluster centers.
2. Assignment: For each data point, calculate the distance (using a distance metric such as
Euclidean distance) from the data point to each centroid, and assign the data point to
the nearest centroid. This forms K clusters where each data point belongs to the cluster
of the nearest centroid.
3. Update: After all data points are assigned to clusters, update the centroids by calculating
the mean (centroid) of all the data points in each cluster. This moves the centroids to the
center of the data points within each cluster.
4. Iteration: Repeat steps 2 and 3 iteratively until convergence is reached. Convergence is
usually determined by a predefined criterion, such as a maximum number of iterations or
when the centroids' movement falls below a threshold.
5. Finalization: Once the algorithm converges, the final centroids represent the final cluster
centers. Each data point belongs to the cluster with the nearest centroid, and the
centroids' coordinates can be used as representative features or prototypes for each
cluster.
It's important to note that K-means algorithm can converge to local optima, meaning that the
result can be different depending on the initial placement of centroids. To mitigate this, it's
common to run K-means multiple times with different initializations and choose the best
clustering result based on a predefined evaluation metric, such as silhouette score or sum of
squared distances.
