# ML-InSAR-denoising

A demonstration of multiple Machine Learning methods to remove Tropospheric Signals from InSAR data. The work has been developed for the NASA SpaceApps Hackathon 2022 - InSAR Change Detectives!

## OpenCV Fast NL Means Algorithm

Non-local means is an algorithm in image processing for image denoising. Unlike "local mean" filters, which take the mean value of a group of pixels surrounding a target pixel to smooth the image, non-local means filtering takes a mean of all pixels in the image, weighted by how similar these pixels are to the target pixel. This results in much greater post-filtering clarity, and less loss of detail in the image compared with local mean algorithms.

The non-local means algorithm replaces the value of a pixel by an average of a selection of other pixels values: small patches centered on the other pixels are compared to the patch centered on the pixel of interest, and the average is performed only for pixels that have patches close to the current patch. As a result, this algorithm can restore well textures, that would be blurred by other denoising algorithm. When fast_mode is True a faster algorithm employing uniform spatial weighting on the patches is applied. 

#### Results

Common arguments are:

* h : parameter deciding filter strength. Higher h value removes noise better, but removes details of image also. (experimented with 10 & 11)
* hForColorComponents : same as h, but for color images only. (normally same as h)
* templateWindowSize : should be odd. (recommended 7)
* searchWindowSize : should be odd. (recommended 21)

<img width="1001" alt="Screenshot 2022-10-02 at 20 13 38" src="https://user-images.githubusercontent.com/34877328/193472007-716cc409-b3e8-42c1-9417-7d6d2722e7fe.png">


## Reference
* https://www.askpython.com/python/examples/denoising-images-in-python
* https://docs.opencv.org/3.4/d5/d69/tutorial_py_non_local_means.html
* https://scikit-image.org/docs/stable/auto_examples/filters/plot_nonlocal_means.html
* A. Buades, B. Coll and J. . -M. Morel, "A non-local algorithm for image denoising," 2005 IEEE Computer Society Conference on Computer Vision and Pattern Recognition (CVPR'05), 2005, pp. 60-65 vol. 2, doi: 10.1109/CVPR.2005.38.
