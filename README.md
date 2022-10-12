### Motion Saliency Segmentation using 3D Gray-Code Kernels
#### Elena Nicora, MaLGa Centre, Università degli Studi di Genova (2022) 
-----------------------------------------------------------------------------------------
This code implements the entire **Attentive Module** of 

> "On the use of efficient projection kernels for motion-based visual saliency estimation."
Nicora, Elena, and Nicoletta Noceti. Frontiers in Computer Science: 67.

> ["Efficient Projections for Salient Motion Detection and Representation"](http://hdl.handle.net/11567/1091835)
Nicora Elena, Università degli Studi di Genova (2022)
-----------------------------------------------------------------------------------------
The algorithm requires in input a pair of maps, max_pooling and avg_pooling, and returns a refined segmentation of the moving object in the scene.

**IMPORTANT:** Pooling maps required in input are computed using __[3D Gray-Code Kernels](https://github.com/Malga-Vision/3DGrayCodeKernels)__ (see paper cited above for more information)

For each pair of pooling maps we can identify the following steps:
1. **Otsu's adaptive thresholding**: returns a coarse segmentation (GCKsA) based only on max_pooling
1. Refinement of GCKsA using **minima and maxima** of the avg_pooling: here we discard connected components of GCKsA smaller than a 5x5 patch and those that do not correspond to areas where there are local extrema
1. **Identification of "present" and "past"** values by using two thresholds (delta_1 and delta_2) over the avg_pooling
1. Composition of the **refined segmentation (GCKsR)** subtracting the pixels corresponding to the "past" and adding the "present" pixels to consolidate the final result.

Note that every step contains several morphological operations in order to produce a visually pleasing segmentation with the least number of holes and artifacts.

Users can tune variables **delta_1 and delta_2**.

We assume that values or the avg_pooling near zero correspond to the past positions and values near 1 correspond to the present. But notice that in some cases it is the opposite so the user needs to invert the name of the variables in first_ref (first refinement that subtracts the "past") and second_ref (second refinement that adds the "present").

