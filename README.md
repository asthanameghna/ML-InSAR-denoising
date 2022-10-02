# ML-InSAR-denoising

A demonstration of multiple Machine Learning methods to remove Tropospheric Signals from InSAR data. The work has been developed for the NASA SpaceApps Hackathon 2022 - InSAR Change Detectives!

## Fast NL Means Algorithm (OpenCV)

Non-local means is an algorithm in image processing for image denoising. Unlike "local mean" filters, which take the mean value of a group of pixels surrounding a target pixel to smooth the image, non-local means filtering takes a mean of all pixels in the image, weighted by how similar these pixels are to the target pixel. This results in much greater post-filtering clarity, and less loss of detail in the image compared with local mean algorithms.

The non-local means algorithm replaces the value of a pixel by an average of a selection of other pixels values: small patches centered on the other pixels are compared to the patch centered on the pixel of interest, and the average is performed only for pixels that have patches close to the current patch. As a result, this algorithm can restore well textures, that would be blurred by other denoising algorithm. When fast_mode is True a faster algorithm employing uniform spatial weighting on the patches is applied. 

#### Results

Common arguments are:

* h : parameter deciding filter strength. Higher h value removes noise better, but removes details of image also. (experimented with 10 & 11)
* hForColorComponents : same as h, but for color images only. (normally same as h)
* templateWindowSize : should be odd. (recommended 7)
* searchWindowSize : should be odd. (recommended 21)

<img width="1001" alt="Screenshot 2022-10-02 at 20 13 38" src="https://user-images.githubusercontent.com/34877328/193472007-716cc409-b3e8-42c1-9417-7d6d2722e7fe.png">

## Dictionary Learning (scikit-learn)

Dictionary learning is a sparse encoding schema. Where encoding schema is lossless from an analytical implementation perspective. And it is guaranteed global optimum encoding from a numerical implementation perspective because the analytical re-formulation is a convex optimization problem.

In our experiment, the dictionary is fitted on the distorted left half of the image, and subsequently used to reconstruct the right half. Note that even better performance could be achieved by fitting to an undistorted (i.e. noiseless) image, but here we start from the assumption that it is not available.

A common practice for evaluating the results of image denoising is by looking at the difference between the reconstruction and the original image. If the reconstruction is perfect this will look like Gaussian noise.

It can be seen from the plots that the results of Orthogonal Matching Pursuit (OMP) with two non-zero coefficients is a bit less biased than when keeping only one (the edges look less prominent). It is in addition closer from the ground truth in Frobenius norm.

The result of Least Angle Regression is much more strongly biased: the difference is reminiscent of the local intensity value of the original image.

Thresholding is clearly not useful for denoising, but it is here to show that it can produce a suggestive output with very high speed, and thus be useful for other tasks such as object classification, where performance is not necessarily related to visualisation.
<p float="center">
  <img width="452" alt="Screenshot 2022-10-02 at 21 05 31" src="https://user-images.githubusercontent.com/34877328/193473990-4981c3e8-e140-4473-ba53-91a0bac42d55.png">
  <img width="383" alt="Screenshot 2022-10-02 at 21 05 50" src="https://user-images.githubusercontent.com/34877328/193474006-84639cee-2e7e-4cc5-93fe-3ca22f5c3c1d.png">
</p>


## Reference
* https://www.askpython.com/python/examples/denoising-images-in-python
* https://docs.opencv.org/3.4/d5/d69/tutorial_py_non_local_means.html
* https://scikit-image.org/docs/stable/auto_examples/filters/plot_nonlocal_means.html
* A. Buades, B. Coll and J. . -M. Morel, "A non-local algorithm for image denoising," 2005 IEEE Computer Society Conference on Computer Vision and Pattern Recognition (CVPR'05), 2005, pp. 60-65 vol. 2, doi: 10.1109/CVPR.2005.38.
* https://medium.com/analytics-vidhya/what-dictionary-learning-actually-is-812d264e9646
* https://scikit-learn.org/stable/auto_examples/decomposition/plot_image_denoising.html
* https://scikit-learn.org/stable/modules/decomposition.html#dictionary-learning
