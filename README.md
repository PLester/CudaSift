# CudaSift -- SIFT features with CUDA

This is the fourth version of a SIFT (Scale Invariant Feature Transform) implementation using CUDA for GPUs from NVidia. The first version is from 2007 and GPUs have evolved since then. This version is slightly more precise and considerably faster than the previous versions and has been optimized for Tesla K40 using larger images.

On a Tesla K40 GPU the code takes about 5.3 ms on a 1280x960 pixel image and 6.4 ms on a 1920x1080 pixel image, while the third version required respectively 11.2 ms and 14.5 ms. An additional 1.5 ms and 1.0 ms is needed for image transfers from CPU to GPU. There is also code for brute-force matching of features and homography computation that takes about 2.5 ms and 3 ms for two sets of around 1250 SIFT features each.

The code relies on CMake for compilation and OpenCV for image containers. OpenCV can however be quite easily changed to something else. The code can be relatively hard to read, given the way things have been parallelized for maximum speed.

The code is free to use for non-commercial applications. If you use the code for research, please refer to the following paper.

M. Bj�rkman, N. Bergstr�m and D. Kragic, "Detecting, segmenting and tracking unknown objects using multi-label MRF inference", CVIU, 118, pp. 111-127, January 2014.


## Benchmarking:

Computational cost (in milliseconds) on different GPUs (latest benchmark marked with *):

|      |                 | 1280x960 | 1920x1080 |  GFLOPS  | Bandwidth | Matching |
| Pascal  |  GeForce GTX 1060   |   2.6* |     3.8* |	   3855  |    192  |   1.3* |
| Maxwell |  GeForce GTX 970    |   5.0  |     6.5  |    3494    |  224    |   1.6  |
| Maxwell | GeForce GTX 750 Ti  | 10.6   |   14.7   |    1306    |   86    |   3.2  |
| Kepler  | Tesla K40           |  5.3   |    6.4   |    4291    |  288    |   2.4  |
| Kepler  | GeForce GTX TITAN   |  4.6   |    5.8   |    4500    |  288    |   2.5  |

Matching is done between two sets of 1050 and 1202 features respectively. 
 
The latest improvements involve a slight adaptation for Pascal, changing from textures to global memory (mostly through L2) in the most costly function LaplaceMulti. The new medium-end card GTX 1060 is impressive indeed. It will be interesting to see the performance on the NVidia Titan X and other Pascal cards.  